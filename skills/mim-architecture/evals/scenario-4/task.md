# Complete the User Management Module

## Problem/Feature Description

We are building a `User Management` capability. The core business rules and logic are already written in a `UserService`. This service depends on an `IUserStore` interface to manage user data, but we haven't implemented the actual technical details for saving and fetching users yet.

The company is focused on modular design and wants to ensure that the technical infrastructure (like databases and API frameworks) is separated from the business logic. This separation allows us to change the database or the web framework without touching the business rules.

Your task is to implement the missing technical details for this feature. You need to provide a concrete database implementation for the user store and a clear way for the rest of the application to interact with this module. Finally, you need to show how everything is wired together to make it functional.

## Input Files

The following file is provided as a reference:

=============== FILE: src/UserManagement/UserService.ts ===============
import { IUserStore, User } from "./Interfaces";

export class UserService {
  constructor(private repo: IUserStore) {}

  async registerUser(userData: any): Promise<User> {
    const user = new User(userData.name, userData.email);
    await this.repo.save(user);
    return user;
  }
}

=============== FILE: src/UserManagement/Interfaces.ts ===============
export interface IUserStore {
  save(user: User): Promise<void>;
  findById(id: string): Promise<User | null>;
}

export class User {
  constructor(public name: string, public email: string, public id?: string) {}
}

## Output Specification

- Provide a directory structure for this feature.
- Implement the concrete database repository for the user store (e.g., using PostgreSQL, MongoDB, or an in-memory solution).
- Provide a clear entry point (e.g., a Controller, Listener, or a factory function) that allows other parts of the system to use the feature.
- Show how the technical components are connected to the business service.
- Maintain a strict separation between the business logic and the technical implementation.
