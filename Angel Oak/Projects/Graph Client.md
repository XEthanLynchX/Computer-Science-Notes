## Overview 

**GraphClient** is a client-side service designed to simplify integration with Microsoft 365 services via the Microsoft Graph API. It provides a clean abstraction for performing common Office 365 tasks such as retrieving user information, sending emails, and managing calendar events. The goal was to reduce repetitive code and streamline interactions with Microsoft services across multiple projects.

--- 

## Role

- Co-designed and built **GraphClient** from [[GraphPrototype]]
- Documented use cases and potential implementation patterns for team-wide adoption
- Conducted thorough testing both locally and in production environments to ensure reliability
- Developed automated test suites using Jest to safeguard future development and refactoring
- Audited GitHub repositories to identify existing GraphClient implementations and promote reuse

---

## Impact

- Successfully integrated into **5+ projects**, handling email operations post-task completion
- Reduced development time for Email tasks by creating package for Email Use in development
- Improved code maintainability and readability across services using GraphClient
- Enabled faster onboarding for new developers by providing clear documentation and reusable patterns
- Increased confidence in production deployments through robust automated testing

---

## Tech Stack 

- **Languages & Frameworks:** TypeScript, Node.js
- **Testing:** Jest
- **APIs:** Microsoft Graph API