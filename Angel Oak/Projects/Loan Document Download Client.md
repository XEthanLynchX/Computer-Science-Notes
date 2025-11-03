## Overview

Refactored and abstracted utility calls—specifically `ApiContext` and `ServiceRequest`—to create a standardized interface for service communication with an external Durable Function responsible for loan document downloads. This effort focused on improving code clarity, consistency, and long-term maintainability.

---

## Role

As an Associate Developer, I identified inconsistencies in how service calls were being made and took initiative to abstract these utilities into a reusable, well-documented pattern. I ensured the new structure aligned with existing architecture while simplifying integration points for future services.

---

## Impact

- Standardized service communication by abstracting `ApiContext` and `ServiceRequest`, reducing duplication and improving reliability.
- Enhanced code readability and maintainability across the Lockscreen Functions app by introducing cleaner patterns and removing ad-hoc implementations.
- Delivered thorough inline documentation and usage examples to support team adoption and ease onboarding for future developers.