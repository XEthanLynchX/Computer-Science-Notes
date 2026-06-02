## 1. High-Level Goal

- Intercept messages currently flowing from **Cornucopia → CMMP**
- Filter them based on rules
- Send **qualified data to CORTX API** for BOL (Bill of Lading) generation
- Ensure system is **resilient to outages and retries properly**

## 2. Architecture Overview 

### Message Capture

**What it does**

- Copies messages from existing integration (you don’t change the source flow)
- Stores them in new queues for processing

**Key ideas**

- Each location gets:
    - Its own **topic**
    - Its own **queue + route**
- EMS bridge duplicates messages → sends to your system

Output: Raw messages per location queue

---

### Filter Process (Decision + Transform)

**What it does**
- Decides: “Should this message go to CORTX?”
- If yes → transforms and forwards
- If no → drop it

**Steps**

1. Listen to location-specific queue
2. Track metadata:
    - ID
    - DocType
    - Alternate ID
3. Apply business filtering rules
4. If valid:
    - Fetch extra data from Cornucopia views
    - Transform to required JSON format
    - Base64 encode payload
    - Wrap it in a standardized message
5. Publish to **CORTX Delivery Topic**

Output: Clean, validated, encoded messages ready for API

---

### Delivery Process (Send to API)

**What it does**

- Takes prepared messages and sends them to CORTX (via SF wrapper API)

**Steps**

1. Listen to delivery topic
2. Extract:
    - Object type (ScaleTicket / SqlObj)
    - Payload
3. Get Azure JWT token
4. Call correct API endpoint
5. Handle errors:
    - Retry on connection failures
    - Send failures to retry queue

Output: API calls executed or sent for retry

---

### Retry Process 

**What it does** Handles failures and ensures delivery isn’t lost.

**Logic**
- If `RetryCount < MaxRetries` → Increment and retry later
- If `RetryCount == MaxRetries` → Send to Dead Letter Queue (DLQ) → Trigger notification

**Extras**

- Runs every 15 minutes (batched retries)
- Sends aggregated error emails

Output: Reliable delivery or controlled failure