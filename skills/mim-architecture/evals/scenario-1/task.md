# Refactor User Registration for Maintainability

## Problem/Feature Description

The user registration logic in our application has become difficult to maintain because its components are scattered across top-level global technical layer folders. Adding a small field or changing a rule requires jumping between `src/controllers`, `src/services`, and `src/repositories`, leading to high cognitive load and fragmentation.

The current structure looks like this:

```
src/
├── controllers/
│   └── UserController.ts
├── services/
│   └── UserRegistrationService.ts
└── repositories/
    └── UserRepository.ts
```

This "horizontal layering" makes it hard to see the complete flow of "User Enrollment" at a glance. It also causes tight coupling, as these global layer folders are shared by multiple unrelated features.

Your task is to refactor this "User Registration" logic into a single cohesive module (e.g., `UserEnrollment`). The goal is to move towards a structure where each business capability is self-contained, keeping everything relevant to a feature together while ensuring the core business rules are isolated from technical implementation details like database access.

## Input Files

The following files are provided as inputs. Extract them before beginning.

=============== FILE: src/controllers/UserController.ts ===============
import { UserRegistrationService } from "../services/UserRegistrationService";

export class UserController {
  constructor(private service: UserRegistrationService) {}

  async register(req: any, res: any) {
    const user = await this.service.register(req.body);
    res.status(201).json(user);
  }
}

=============== FILE: src/services/UserRegistrationService.ts ===============
import { UserRepository } from "../repositories/UserRepository";

export class UserRegistrationService {
  constructor(private repo: UserRepository) {}

  async register(data: any) {
    // business logic here
    return this.repo.save(data);
  }
}

=============== FILE: src/repositories/UserRepository.ts ===============
export class UserRepository {
  async save(user: any) {
    // save to DB
    return user;
  }
}

## Output Specification

- Refactor the code into a new, single-feature module.
- Organize the code within the module to separate business logic from technical infrastructure.
- Ensure the business logic has zero compile-time dependencies on concrete persistence implementations.
- Show the final directory structure and the refactored code for each part of the module.
