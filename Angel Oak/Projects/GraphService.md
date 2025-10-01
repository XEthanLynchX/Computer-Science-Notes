## Overview

**GraphService** is a Durable Azure Function-based service built to perform Microsoft-exclusive actions using the custom **Angel Oak GraphClient** package. It orchestrates email operations and user data retrieval through Microsoft Graph API, with robust error handling and poison message management via Blob Storage. The service supports both HTTP and Postman-based testing workflows and integrates seamlessly with internal systems.

---

## Role

- Developed and maintained **GraphService** to abstract Microsoft Graph API operations
- Integrated the custom `@angeloakcompanies/graph-client` package for streamlined communication with Microsoft 365 services
- Implemented Durable Functions orchestrators and activities for sending emails and retrieving user data
- Designed and documented local development workflows using Azure Functions Core Tools and Azurite
- Built poison message handling logic to capture and store failed email operations in Blob Storage
- Configured Application Insights alerts for poison message detection and diagnostics
- Created Postman collections for testing and validation of service endpoints
- Managed environment configuration and secure access to Azure resources

---

## Impact

- Enabled reliable and scalable email orchestration across multiple internal applications
- Reduced operational failures by implementing poison message handling and alerting
- Improved developer productivity through clear documentation and local testing support
- Enhanced observability and incident response via Application Insights integration
- Supported reprocessing of failed messages, reducing manual intervention and data loss
- Strengthened security and maintainability through centralized environment variable management

---

## Tech Stack

- **Languages & Frameworks:** TypeScript, Azure Durable Functions
- **Testing & Dev Tools:** Postman, Azurite, Azure Storage Explorer
- **Azure Services:** Microsoft Graph API, Blob Storage, Queue Storage, Application Insights
