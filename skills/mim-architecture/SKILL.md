---
name: mim-architecture
description: Define module boundaries, structure infrastructure layers, implement dependency inversion, and create sociable unit tests using MIM (Module-Infrastructure-Module) architecture and modular design principles. Use this skill when the user mentions "MIM", "Module-Infrastructure-Module", "Screaming Architecture", or asks for advice on modular design, high cohesion, low coupling, or sociable unit testing.
---

# MIM Architecture & Modular Design

MIM organizes code around business processes rather than technical layers, splitting logic into Business and Infrastructure modules.

## Core Principles

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
- **Handling Errors with Fakes:** To test error scenarios, configure your fakes to fail. Avoid using `jest.fn().mockRejectedValue()`. Instead, add a `shouldFail` flag or a specific error response to your fake class.

### Example: Fake with Error Support
```typescript
class FakeIoTGateway implements IIoTGateway {
  public shouldFail = false;
  async send(version: string, deviceId: string) {
    if (this.shouldFail) throw new Error("Connection failed");
    // ... success logic
  }
}

test("should handle gateway failure", async () => {
  const gateway = new FakeIoTGateway();
  gateway.shouldFail = true; // Configure fake to fail
  const service = new FirmwareDispatcher(new FakeFirmwareRepo(), gateway);
  await expect(service.dispatch("v1", "d1")).rejects.toThrow("Connection failed");
});
```

- **Refactoring Safety:** Because tests target the public API rather than internal classes, developers can refactor internal code (split or merge classes) without breaking the tests.

## Implementation Guide

1. **Identify the Process:** Choose a business process or capability (e.g., `FirmwareDispatcher`).
2. **Create the Business-Module (BM):**
   - Define the public API (Service or Command Handler).
   - Define interfaces for required external dependencies (Persistence, APIs).
   - Implement the domain logic and business rules.
   - **Validation:** Ensure the BM has zero imports from infrastructure or external libraries (e.g., TypeORM, Axios).
   - **Correction:** If the BM imports infrastructure code, extract an interface for that dependency, keep the interface in the BM, and move the implementation to the IM.
3. **Create the Infrastructure-Module (IM):**
   - Implement the interfaces defined in the BM (e.g., SQL implementation for repository).
   - Set up entry points (HTTP Controllers, Message Listeners).
   - Handle Dependency Injection to inject IM implementations into the BM.
   - **Validation:** Verify that the IM depends on the BM, never the other way around. Use tools like `dependency-cruiser` or manual inspection of `import` statements to confirm the unidirectional dependency.
   - **Correction:** If a circular dependency exists, move the shared logic to a more foundational module or the BM if it's pure business logic.

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

## References
- [MIM Overview](./references/mim-overview.md)
- [Modular Design Principles](./references/modular-design.md)
