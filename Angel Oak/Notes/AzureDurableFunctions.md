### Application Insights Logging

Application Insights should be actively capturing logs, traces, and exceptions. This is crucial for monitoring orchestrator and activity behavior. You can use **custom KQL queries** to filter logs, identify poison message events, and track performance metrics. Make sure your `APPLICATIONINSIGHTS_CONNECTION_STRING` is correctly configured in the environment.

---

### Poison Message Handling

Durable Functions don't automatically move failed orchestrations to poison queues. Instead, you should handle failures inside the orchestrator using `try/catch` blocks and call a dedicated activity (e.g., `PoisonMessageHandler`) to persist failed inputs to **Blob Storage**. This ensures traceability and allows for manual recovery.

In Azure, navigate to your **Storage Account > Containers > poison-messages** to inspect stored blobs. Each blob contains metadata such as timestamp, invocation ID, and function name, which helps in diagnosing issues.

---

### Alerting & Notifications

To stay ahead of failures, configure **Application Insights alerts** based on specific error messages or patterns (e.g., `"PoisonEmailHandler triggered"`). Alerts can be routed through **Azure Action Groups** to notify teams via email, SMS, Teams, or webhooks. Suppression settings help avoid alert fatigue by limiting duplicate notifications.

---

### Reprocessing Poison Messages

If a message fails and is stored in Blob Storage, you can manually download and edit the JSON blob to fix the issue (e.g., invalid email address). Once corrected, requeue the message using Postman or the GraphClient. The system will treat it as a new message and attempt to process it again.

---

### Maintenance Considerations

- **Purge orchestration history** periodically to manage storage costs, especially for high-volume apps.
- **Monitor retry behavior** to avoid duplicate orchestrations caused by aggressive dequeue settings.
- **Log critical errors** using `context.error` to ensure visibility in monitoring tools.