# Refactor to Cohesive Modules

## Problem/Feature Description

An existing application for user registration has become difficult to maintain because the logic is split across multiple top-level folders based on technical roles. The current structure is as follows:

```
src/
├── controllers/
│   └── UserController.ts
├── services/
│   └── UserRegistrationService.ts
└── repositories/
    └── UserRepository.ts
```

This "horizontal layering" makes it hard to see the complete flow of a single business process like "User Enrollment." It also leads to tight coupling between different features as they share the same global layer folders.

Your task is to refactor this "User Registration" logic into a single cohesive "User Enrollment" module using the MIM architecture. All components related to enrolling a user (API, business logic, persistence) should be bundled together in a way that reflects the business capability.

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

- Refactor the code into a new, single-feature module (e.g., `UserEnrollment`).
- Use the MIM (Module - Infrastructure - Module) split within this module.
- Ensure the business logic has zero compile-time dependencies on the repository implementation.
- Show the final directory structure and the refactored code for each file.
