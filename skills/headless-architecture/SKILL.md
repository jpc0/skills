---
name: headless-architecture
description: Use this skill whenever the user discusses application architecture, clean architecture, separating business logic from UI, or cross-platform/WASM-based development. It enforces the 'Headless Core & Passive View' pattern, ensuring that the 'Core' (Business Logic) is platform-agnostic and the 'View' (UI) is a 'Humble Object' that only reflects state.
---

# Headless Core & Passive View Architecture

This skill implements a strict separation between the **Application** (Business Logic, State, Rules) and the **Presentation** (UI, Inputs, Rendering).

## 1. Core Philosophy
* **The "Core"**: This is the application. It holds the "Source of Truth," is platform-agnostic, and contains zero UI code.
* **The "View"**: This is a dumb rendering layer ("Humble Object") that strictly reflects the state provided by the Core.

## 2. The Layers

### Layer A: The Domain (Core)
* **Responsibility**: Defines the "Rules of the Game."
* **Content**: Pure entities, validation logic, and calculations.
* **Constraint**: Must compile without any UI framework dependencies.

### Layer B: The State Machine (Core)
* **Responsibility**: Manages application lifecycle and data flow.
* **Pattern**: Receives **Actions/Commands** -> Mutates **State** -> Emits **Snapshots/ViewModels**.
* **Handling Concurrency**: Implements "Codec" logic for real-time data (buffering, jitter compensation) before exposing clean state to the View.

### Layer C: The Bridge (Interface)
* **Responsibility**: Serialization and Transport.
* **Mechanism**:
    * **Input**: Strongly typed Commands (e.g., `SubmitForm { data }`).
    * **Output**: Read-only ViewModels (e.g., `DashboardState { graphs: [...] }`).
* **Rule**: Never pass complex internal objects (like DB connections) to the View. Pass simple Data Transfer Objects (DTOs).

### Layer D: The Passive View (UI)
* **Responsibility**:
    1.  **Render**: Draw pixels based *only* on the current ViewModel.
    2.  **Capture**: Detect user intent and dispatch Commands to the Core.
* **Validation**: UI-specific validation (like "field is required" or simple regex for instant feedback) is allowed for UX improvement, but all business-critical validation MUST live in the Core.

## 3. Abstract Implementation Patterns

### Pattern 1: The Validator (Forms & Input)
* **View**: Holds "Draft State" (raw strings/inputs).
* **Core**: Exposes `validate(draft)` -> returns `Errors`.
* **Flow**: View calls `validate()` on change -> Core returns status -> View updates UI.

### Pattern 2: The Codec (Real-time Sync)
* **Core**: Acts as a buffer for noisy packets, applies deltas, handles sync/interpolation.
* **View**: Asks Core for "Current Frame" inside a render loop.

### Pattern 3: The Pointer (Heavy Assets)
* **View (Control Plane)**: Browses lightweight Metadata/IDs.
* **Core (Data Plane)**: Resolves ID to resource, handles streaming/decoding.

## 4. Developer Instructions
When generating code or designing systems:
1.  **Monorepo Structure**: Default to a monorepo structure (e.g., `/core` for logic, `/ui` for presentation, and `/shared` or `/bridge` for interfaces).
2.  **Start with the Core**: Always define the Core structs, logic, and state first.
3.  **Define the Interface**: Explicitly define the FFI boundary, WASM bindings, or API contracts between Core and View.
4.  **Build the View Last**: Write the Frontend code assuming the Core library already exists and works perfectly.
5.  **Enforce Boundaries**: If the user asks to put business logic in a UI component (e.g., `if user.is_admin`), **refuse** and move it to the Core, exposing a property in the ViewModel instead.
