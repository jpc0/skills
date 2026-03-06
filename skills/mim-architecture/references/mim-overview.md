# MIM Architecture Overview

MIM (Module - Infrastructure - Module) is a pragmatic architectural pattern designed to simplify application development by focusing on modular design and decoupling business logic from technical infrastructure.

## Key Goals
- **Maintainability:** Easy to change internal logic without affecting other modules.
- **Understandability:** Folders and modules reflect business processes ("Screaming Architecture").
- **Testability:** High confidence through sociable unit tests with minimal boilerplate.
- **Replaceability:** Individual modules can be easily refactored or extracted into microservices.

## Architectural Layers
Unlike traditional horizontal layers (Domain, Infrastructure, Application), MIM uses vertical "Module" stacks.

### Business-Module (BM)
- **Role:** The core of the feature.
- **Content:** Services, Domain Models, Value Objects, and **Interface Definitions**.
- **Rule:** No knowledge of databases, external APIs, or frameworks.
- **Testing:** Sociable unit tests using fakes for interfaces.

### Infrastructure-Module (IM)
- **Role:** The technical glue.
- **Content:** SQL implementations, HTTP controllers, Message bus listeners, DI configuration.
- **Rule:** Depends on the BM and implements its interfaces.
- **Testing:** Limited integration tests to verify the wiring with real resources.

## Dependency Flow
1. **User Request** -> Hits Infrastructure-Module (Controller).
2. **IM** -> Calls Business-Module (Service).
3. **BM** -> Executes logic, calls its own Interfaces (e.g., `IRepository`).
4. **IM** -> Provides the implementation for the BM's Interfaces.

This flow ensures that the core business logic remains isolated from technical details, following the Dependency Inversion Principle.
