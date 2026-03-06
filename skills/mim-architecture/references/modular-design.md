# Introduction to Modular Design

Modular design is the foundation of the MIM architecture. It focuses on how to structure code into independent, cohesive, and encapsulated units.

## 1. High Cohesion
Elements that change together should stay together.
- **Problem:** Splitting a single feature (e.g., "User Login") into separate layers (Controllers, Services, Models) in different folders. This makes the code harder to find and change.
- **MIM Solution:** Bundle all components of a feature into a single module. The controller, service, and even the persistence logic for "User Login" should be in the same place.

## 2. Low Coupling
Modules should have minimal dependencies on each other.
- **Goal:** A change in the "Menus" module should not require a change in the "Orders" module.
- **Avoid:** Direct database access between modules, shared web APIs that expose internal structures, and complex dependency chains.
- **Communication:** Modules should interact via clear, stable public APIs or message buses.

## 3. Encapsulation & Information Hiding
Expose only what is necessary and hide the internal implementation.
- **Interface vs. Implementation:** A module should expose a clear, well-defined interface (the "What") and hide how it works internally (the "How").
- **Benefit:** Allows you to refactor the internal code (e.g., split a large class into smaller ones) without breaking the rest of the system, as long as the public API remains unchanged.

## Definition of a Module
A module is a representation of a **business capability or process**.
- It is a "gray area" in design—larger than a class but smaller than the entire system.
- It should be **self-contained**: It manages its own state and data.
- It should be **replaceable**: You can swap one implementation of a module for another if they share the same public interface.

## Implementation Ideas
- **Folders by Feature:** Organize your project directory by feature/process names (`Orders`, `Inventory`, `Shipping`) rather than by technical roles (`Controllers`, `Services`).
- **Public API:** Each module should have a clear entry point (e.g., a specific Service class or a set of Command Handlers) that defines its public interface.
- **Zero Cross-Module Data Access:** Prevent modules from reaching into each other's databases or internal classes. Use the public API for all cross-module communication.
