# Cornucopia → CORTX Pipeline: Explanation & Design

---

## Part 1 — Understanding the Current System

### What "Cornucopia" Likely Is

Based on this codebase, **Cornucopia is a plant-floor scale/weighing system**. It captures physical events at a manufacturing or shipping facility — trucks driving onto a scale, gross/tare/net weights, seal numbers, lot numbers, scale sequence numbers, timestamps, etc.

Looking at [index.js](vscode-file://vscode-app/Applications/Visual%20Studio%20Code.app/Contents/Resources/app/out/vs/code/electron-browser/workbench/workbench.html) and [cornucopia.js](vscode-file://vscode-app/Applications/Visual%20Studio%20Code.app/Contents/Resources/app/out/vs/code/electron-browser/workbench/workbench.html), every payload describes a weigh event tied to:

- A **CMMP order number** (the business document the weigh-up belongs to)
- A **plant location code**
- Weights, scale times, seals, truck/trailer ID

So conceptually:

> Cornucopia = the system of record for **what happened on the scale**.  
> CMMP = the system of record for **the order / load / shipment**.

### How It Talks to CMMP and Downstream

Cornucopia historically pushes scale events into **CMMP** (a legacy order/load management system). CMMP then drives downstream things like Bill of Lading (BOL) generation, inventory movement, and financials.

The plant-api you're in today is a _parallel_ consumer — when Cornucopia POSTs a scale event, this API:

1. Looks up the Load or Internal Delivery in Salesforce (CORTX) by CMMP order number
2. Validates plant + weights
3. Updates the Salesforce record with scale info
4. Optionally creates Load Seals

If Salesforce is down, it **stages** the payload to Postgres and a scheduler retries later ([checkStagedData.js](vscode-file://vscode-app/Applications/Visual%20Studio%20Code.app/Contents/Resources/app/out/vs/code/electron-browser/workbench/workbench.html)).

### EMS Bridge, Topics, Queues, Routing — In Plain English

These terms come from **TIBCO EMS** (Enterprise Message Service), a JMS-style message broker common in industrial/enterprise integrations.

|Term|Plain Meaning|
|---|---|
|**EMS Bridge**|A built-in "tee" inside the broker. It copies messages arriving on Source A to Destination B automatically — no application code involved. Perfect for _non-disruptively duplicating_ a stream.|
|**Topic**|Publish/subscribe channel. One publisher, many subscribers — each gets a copy.|
|**Queue**|Point-to-point channel. One producer, one consumer per message. Used when you want exactly-one processing.|
|**Routing rules**|Configuration that says "messages matching X selector go to queue Y." Lets you fan a topic out to per-location queues.|
|**Message transformation**|Reshaping a payload (XML → JSON, rename fields, enrich with lookups) before forwarding.|
|**Filtering**|Dropping messages that don't meet business rules so downstream systems aren't flooded.|
|**Delivery pipeline**|The end-to-end chain: capture → transform → deliver → confirm/retry.|

### Mental Model of Today
[Scale Hardware] → [Cornucopia] → [EMS Topic] → [CMMP consumer] → CMMP DB → BOL
                                       │
                                       └─(also)─→ [HTTP POST to cortx-plant-api] → Salesforce

---

## Part 2 — Designing the New Pipeline

### High-Level Goal Recap

- **Tap** (don't redirect) the Cornucopia → CMMP stream
- **Filter** to only messages that should generate a BOL in CORTX
- **No staging in Salesforce** — only push real updates when something changes
- **Deliver** qualifying messages to a CORTX BOL API
- **Retry + DLQ** for resilience

### Target Architecture (Text Diagram)

                    ┌────────────────────────┐
                    │  Cornucopia Publisher  │
                    └───────────┬────────────┘
                                │ (existing topic)
                                ▼
                ┌───────────────────────────────┐
                │  EMS Topic: cornucopia.events │──────────► CMMP (untouched)
                └───────────────┬───────────────┘
                                │  EMS Bridge (copy)
                                ▼
                ┌───────────────────────────────┐
                │ EMS Topic: cortx.capture.raw  │
                └───────────────┬───────────────┘
                                │  routing rules by location
            ┌───────────┬───────┼────────┬───────────┐
            ▼           ▼       ▼        ▼           ▼
        Q.loc.A     Q.loc.B  Q.loc.C  Q.loc.D    Q.loc.N

            │  (each location queue)
            ▼
   ┌──────────────────────────────┐
   │  Filter + Transform Service  │ (stateless workers, N replicas)
   │  - dedupe by (id, docType)   │
   │  - apply business rules      │
   │  - enrich from Cornucopia DB │
   │  - map → CORTX JSON          │
   │  - base64 + envelope         │
   └──────────────┬───────────────┘
                  ▼
        Topic: cortx.delivery
                  │
                  ▼
   ┌──────────────────────────────┐
   │      Delivery Service        │
   │  - Azure JWT auth            │
   │  - POST to CORTX SF wrapper  │
   │  - classify response         │
   └──────┬──────────────┬────────┘
          │ success      │ retryable failure
          ▼              ▼
       (ack)        Q.retry (delayed)
                        │  after N attempts
                        ▼
                     Q.dlq  ──► Alerting / Ops dashboard

---

### 1. Message Capture Layer

**Goal:** copy the stream safely without touching the producer or CMMP consumer.

**How to do it without disruption:**

- Use an **EMS bridge** at the broker level — zero code on the Cornucopia side, zero risk to CMMP.
- The bridge republishes onto a _new_ topic owned by your team (`cortx.capture.raw`).
- Define **per-location queues** subscribing to that topic with a selector like `locationCode = 'PLT123'`. This isolates blast radius — if one plant misbehaves, others keep flowing.
- Persist raw messages (object storage or a `raw_events` table) before any processing. That gives you replay capability.

**Pattern:** _Claim Check_ + _Tee/Tap_. The bridge is the tap; raw storage is the claim check.

**Tech options:**

- Keep TIBCO EMS if it's already there (lowest friction).
- Or bridge EMS → **Azure Service Bus** / **Kafka** / **Event Hubs** if you want cloud-native tooling. Service Bus gives you native DLQs, sessions, and scheduled retries for free.

---

### 2. Filter + Transform Layer

**Service shape:** **stateless** workers. Any state (dedupe, in-flight tracking) goes in Redis or a small Postgres table. Stateless = trivial horizontal scaling.

**Processing pipeline (pseudocode):**

```js 
async function handleMessage(rawMsg) {
  const meta = extractMeta(rawMsg); // { id, docType, alternateId, locationCode }

  // 1. Idempotency / dedupe
  if (await seenBefore(meta.id, meta.docType)) {
    return ack(); // already processed
  }

  // 2. Schema validation (fail fast)
  const parsed = schemaForDocType(meta.docType).parse(rawMsg.body);

  // 3. Business filter
  if (!shouldForwardToCortx(parsed)) {
    await recordSkip(meta, 'filtered');
    return ack();
  }

  // 4. Enrichment (Cornucopia views / DB lookups)
  const enriched = await enrich(parsed);

  // 5. Transform to CORTX shape
  const cortxPayload = mapToCortx(enriched, meta.docType);

  // 6. Envelope + encode
  const envelope = {
    messageId: uuid(),
    sourceId: meta.id,
    docType: meta.docType,
    locationCode: meta.locationCode,
    timestamp: new Date().toISOString(),
    payloadBase64: Buffer.from(JSON.stringify(cortxPayload)).toString('base64'),
  };

  // 7. Publish
  await publish('cortx.delivery', envelope);
  await markSeen(meta.id, meta.docType);
  return ack();
}
```

**Schema validation & transformation:**

- Use **Zod**, **Joi**, or **AJV (JSON Schema)** — keep schemas versioned per `docType` in a `schemas/` folder.
- Keep transform functions **pure** (input → output, no I/O). Test them in isolation.
- Version the envelope (`schemaVersion: 1`) so you can evolve safely.

---

### 3. Delivery Layer

**Responsibilities:** auth, routing to the right CORTX endpoint, classifying responses.

**Pseudocode:**
```js
async function deliver(envelope) {
  const payload = JSON.parse(Buffer.from(envelope.payloadBase64, 'base64').toString());
  const endpoint = endpointFor(envelope.docType); // BOL, scale-info, etc.

  const token = await tokenCache.get(); // Azure JWT, refreshed lazily
  const res = await httpClient.post(endpoint, payload, {
    headers: {
      Authorization: `Bearer ${token}`,
      'Idempotency-Key': envelope.messageId, // critical
    },
    timeout: 15_000,
  });

  return classify(res); // success | retryable | permanent
}
```

**Best practices:**

- **Idempotency key** = `envelope.messageId`. CORTX should treat duplicates as no-ops. This is the single biggest reliability lever.
- **Token caching** — fetch once, refresh on 401 or near expiry. Don't fetch per request.
- **Timeouts on everything** — connect + request. Never rely on defaults.
- **Connection pooling** (`keep-alive` agent).
- **Circuit breaker** (`opossum` or similar) — if CORTX is down, stop hammering it; fail fast into the retry queue.
- Classify responses:
    - `2xx` → success
    - `408, 425, 429, 5xx` → retryable
    - `4xx` (other) → permanent → DLQ immediately (don't waste retries on bad data)

---

### 4. Retry + DLQ Layer

**Two valid models — pick one:**

|Approach|Pros|Cons|
|---|---|---|
|**Queue-based delayed retry** (Service Bus scheduled messages, or `Q.retry.5m`, `Q.retry.15m`, `Q.retry.1h`)|Event-driven, scales naturally, no polling|Slightly more infra|
|**Scheduled retry job every 15 min** (your existing pattern)|Simple, easy to reason about, batches alerts|Latency floor of 15 min, harder to do exponential backoff cleanly|

For this system I'd recommend **queue-based with exponential backoff** for in-flight failures, plus a **15-min scheduled sweep** as a safety net to catch anything stuck.

**Backoff schedule (example):**
attempt 1: immediate
attempt 2: +30s
attempt 3: +2m
attempt 4: +10m
attempt 5: +1h
attempt 6: DLQ

**DLQ design:**

- One DLQ per location (or per docType) — easier triage.
- Each DLQ entry stores: original envelope, last error, attempt count, first/last failure time, classification.
- A small **DLQ admin UI / CLI** to inspect, replay, or purge.
- **Aggregated alerting:** a job that runs every 15 min, counts new DLQ entries grouped by error class, posts a single Teams/Slack message — not one alert per failure.

**Observability:**

- **Structured logs** with `messageId`, `sourceId`, `docType`, `locationCode`, `attempt`, `latencyMs`.
- **Distributed tracing** (OpenTelemetry) — propagate a trace ID from capture through delivery.
- **Metrics** (Prometheus / App Insights): throughput per location, filter-pass rate, retry rate, DLQ depth, p95 delivery latency, token refresh count.
- **Alerts** on: DLQ depth > threshold, retry rate spike, CORTX 5xx rate, consumer lag.

---

## Part 3 — Implementation Guidance

### Suggested Tech Stack

|Concern|Recommendation|Why|
|---|---|---|
|Messaging|**Azure Service Bus** (or keep TIBCO EMS + bridge to ASB)|Native DLQ, scheduled messages, sessions, sits well with Azure JWT auth you already use|
|Compute|**Azure Functions (Service Bus trigger)** or **Container Apps**|Scale-to-zero for low-volume locations, easy autoscale for busy ones|
|Language|**Node.js / TypeScript**|Matches this repo; TS adds safety for schemas/envelopes|
|Schema validation|**Zod**|Runtime + compile-time types from one definition|
|HTTP client|**undici** or **axios** + **opossum** (circuit breaker)|Performance + resilience|
|Secrets / tokens|**Azure Key Vault** + **MSAL**|Standard Azure JWT flow|
|State (dedupe)|**Azure Cache for Redis** or **Postgres**|Cheap, fast idempotency table|
|Observability|**OpenTelemetry → Azure Monitor / App Insights**|First-class in Azure|
|IaC|**Bicep** or **Terraform**|Per-location queues + bridges defined as code|

### Service Breakdown

Three small services, each independently deployable:

1. **`capture-router`** — owns EMS bridge config + raw event persistence (mostly infra, minimal code).
2. **`filter-transform`** — Service Bus triggered function; one deployment, scales per location via partitioning or per-queue triggers.
3. **`delivery`** — Service Bus triggered function consuming `cortx.delivery`; calls CORTX; handles retry/DLQ classification.

A fourth small piece:

4. **`ops-tooling`** — DLQ inspector, replay CLI, alert aggregator (cron).


### Key Design Patterns

- **Event-driven / pub-sub** — decouples Cornucopia from CORTX.
- **Pipes and Filters** — capture → filter → transform → deliver as composable stages.
- **Claim Check** — raw payload stored once; envelope carries a reference if it grows large.
- **Competing Consumers** — multiple workers per queue for throughput.
- **Circuit Breaker** — protects CORTX and your workers from cascading failure.
- **Outbox / Idempotency Key** — exactly-once _effect_ even with at-least-once delivery.
- **Dead Letter Channel** — first-class failure path, not an afterthought.
- **Strangler Fig** — gradually move logic out of CMMP toward CORTX without a big-bang cutover. You can dial up the filter from "0% to CORTX" to "100%" per location.

### Suggested Rollout

1. Stand up bridge → raw topic → one location's queue. **Log only**, don't deliver. Verify volume and shape.
2. Add filter + transform; write outputs to a "shadow" log, compare against CMMP outcomes.
3. Enable delivery to a CORTX sandbox endpoint.
4. Cut over one quiet location to production CORTX. Monitor DLQ + latency.
5. Expand location-by-location. Each one is just another queue + routing rule.