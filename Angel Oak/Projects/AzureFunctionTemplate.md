## Overview

**Durable-Function-Template** is a TypeScript-based Azure Functions template demonstrating key Durable Function patterns. It includes HTTP and Queue triggers, orchestrators, activity functions, and poison message handling via Blob Storage. The template is designed to accelerate development and testing of resilient, serverless workflows using Azure Durable Functions.

---

## Role

- Designed and implemented a reusable Durable Functions template in TypeScript
- Developed HTTP and Queue-triggered clients to initiate orchestrations
- Built orchestrators that call activity functions with retry logic and error handling
- Implemented poison message handling by persisting failed inputs to Blob Storage
- Created a modular project layout with clear separation of concerns
- Documented setup, configuration, and local development workflows using Azurite and Azure Storage Explorer
- Validated poison queue behavior and orchestrator-level error handling through targeted testing scenarios
- Integrated the template into production services such as **GraphService**

---

## Impact

- Accelerated development of Durable Function-based workflows across multiple internal projects
- Improved reliability and observability of orchestrations through structured error handling and logging
- Enabled consistent handling of failed inputs via Blob Storage, reducing data loss and debugging time
- Provided a clear starting point for new developers working with Azure Durable Functions
- Reduced onboarding time and improved code reuse by offering a well-documented and tested template
- Supported local development and testing with Azurite and Azure Storage Explorer, improving developer productivity
- Template was used consistently where Durable function were being used & created

---

## Tech Stack

- **Languages & Frameworks:** TypeScript, Azure Functions
- **Testing & Dev Tools:** Azurite, Azure Storage Explorer
- **Azure Services:** Durable Functions, Blob Storage, Queue Storage