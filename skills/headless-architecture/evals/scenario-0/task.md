# Account Registration Feature

## Problem/Feature Description

We are building a new application that will eventually run on both web (React) and mobile (React Native). The first feature we need to implement is **User Registration**. 

The registration process requires the user to provide an email and a password. We have some specific rules for security: the password must be at least 8 characters long and must contain at least one digit. If these rules are not met, the user should see appropriate error messages.

Since this feature will be used on multiple platforms, we need to ensure that the core logic is reusable and not tied to any specific UI framework. We want to be able to swap out the React frontend for a mobile native UI later without touching the business rules.

Please design and implement this feature, focusing on the structure of the code to support our cross-platform goals.

## Output Specification

The output should include:
1.  The core business logic (structs/classes/functions) that handles user registration and validation.
2.  The interface or bridge that connects the logic to the user interface.
3.  A placeholder or example UI component that shows how it would interact with the logic.
4.  Organize your files into a logical directory structure that separates the business logic from the presentation.
