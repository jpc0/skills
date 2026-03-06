---
name: mim-architecture
description: Design, develop, and test software systems using the MIM (Module - Infrastructure - Module) architecture and foundational modular design principles. Use this skill when the user mentions "MIM", "Module-Infrastructure-Module", "Screaming Architecture", or asks for advice on modular design, high cohesion, low coupling, or sociable unit testing.
---

# MIM Architecture & Modular Design

MIM is an architectural pattern that organizes code around business processes rather than technical layers. It combines foundational modular design principles with a pragmatic split between business logic and infrastructure.

## Foundational Principles (Modular Design)

Before implementing MIM, ensure the following principles are met:
1. **High Cohesion:** Group elements that change together. Instead of splitting a feature across technical layers (e.g., all controllers in one folder, all services in another), bundle the API, business logic, and persistence for a specific feature into one module.
2. **Low Coupling:** Minimize dependencies between modules. A change in one module (e.g., "Menus") should ideally not require changes in another (e.g., "Orders").
3. **Encapsulation & Information Hiding:** Hide internal implementation details. Expose only a clear public API for interaction, allowing internal refactoring without affecting the rest of the system.

## What is a Module?
In MIM, a module is a representation of a **business capability or process** (e.g., "User Onboarding", "Inventory Management"). It is the "gray area" between classes/functions and system-wide design.

A module is a "black box" that contains everything it needs to fulfill its responsibility. It manages its own data; other modules cannot query its database directly but must go through its public API.

## Core Principles of MIM

### 1. Module Development (BM vs. IM)
For complex logic, a module is split into two parts:
- **Business-Module (BM):** A "black box" containing pure business logic. It has **zero compile-time dependencies** on the outside world (database, network, etc.).
- **Infrastructure-Module (IM):** A subordinate module containing technical code (HTTP handlers, database connectivity, message bus logic). It depends on the BM to implement its interfaces.

### 2. Dependency Inversion via Interfaces
- The **BM defines public interfaces** (Ports) for any external action it needs (e.g., `IRepository`, `IExternalService`).
- The **IM implements these interfaces** (Adapters).
- The IM is responsible for "wiring" the system together using Dependency Injection.

## Testing Strategy: Adaptive Testing
MIM favors **Sociable Unit Tests** over solitary tests with heavy mocking.

- **Sociable Unit Tests:** Test the *entire* Business-Module as a single unit via its public API.
- **Fakes Over Mocks:** Use hand-written "Fakes" (e.g., `InMemoryRepository`) instead of mocking frameworks. Fakes are more reliable, easier to maintain, and lead to less brittle tests.
- **Refactoring Safety:** Because tests target the public API rather than internal classes, developers can refactor internal code (split or merge classes) without breaking the tests.

## Implementation Guide

1. **Identify the Process:** Choose a business process or capability (e.g., `FirmwareDispatcher`).
2. **Create the Business-Module (BM):**
   - Define the public API (Service or Command Handler).
   - Define interfaces for required external dependencies (Persistence, APIs).
   - Implement the domain logic and business rules.
3. **Create the Infrastructure-Module (IM):**
   - Implement the interfaces defined in the BM (e.g., SQL implementation for repository).
   - Set up entry points (HTTP Controllers, Message Listeners).
   - Handle Dependency Injection to inject IM implementations into the BM.

## Examples

### Sociable Test with Fake
```typescript
// FirmwareDispatcher/BM/FirmwareDispatcher.spec.ts
import { FirmwareDispatcher } from "./FirmwareDispatcher";
import { FakeFirmwareRepo } from "./fakes/FakeFirmwareRepo";
import { FakeIoTGateway } from "./fakes/FakeIoTGateway";

test("should dispatch firmware update correctly", async () => {
  const repo = new FakeFirmwareRepo();
  const gateway = new FakeIoTGateway();
  const service = new FirmwareDispatcher(repo, gateway);
  
  await service.dispatch("v1.2.3", "device-456");
  
  const dispatched = await gateway.getDispatchedForDevice("device-456");
  expect(dispatched).toBe("v1.2.3");
});
```

### Dependency Inversion
```typescript
// BM (Business-Module)
export interface IAlarms {
  raise(details: AlarmDetails): Promise<void>;
}

// IM (Infrastructure-Module)
export class PagerDutyAlarms implements IAlarms {
  async raise(details: AlarmDetails) {
    // Technical implementation for PagerDuty...
  }
}
```
