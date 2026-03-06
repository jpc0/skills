# Wiring the User Management Module

## Problem/Feature Description

You are working on a `User Management` module that follows the MIM (Module - Infrastructure - Module) architecture. The Business-Module (BM) for this capability is already complete and contains a `UserService` that depends on a `IUserStore` interface to manage user data.

```typescript
// UserManagement/BM/IUserStore.ts
export interface IUserStore {
  save(user: User): Promise<void>;
  findById(id: string): Promise<User | null>;
}
```

Your task is to implement the technical infrastructure for this module. This involves creating a concrete implementation of the `IUserStore` using a database of your choice (e.g., PostgreSQL or MongoDB) and demonstrating how to wire the module together using Dependency Injection.

## Output Specification

- Provide the implementation for a concrete Infrastructure-Module (IM) for the `User Management` capability.
- Implement the `IUserStore` interface with a technical implementation (e.g., `PostgresUserStore`).
- Show the entry point (e.g., a Controller or Message Listener) that triggers the `UserService`.
- Demonstrate how to "wire" the IM's implementation into the `UserService` using Dependency Injection.
- Ensure that the final directory structure correctly separates the BM and IM and maintains the Dependency Inversion Principle.
- Provide example code for the `UserService` and `User` model to make the solution complete.
