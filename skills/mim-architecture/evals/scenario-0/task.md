# Battery Health Monitoring Scaffolding

## Problem/Feature Description

An IoT company managing a large fleet of industrial batteries needs a new capability to monitor battery health. The system must process incoming telemetry readings (voltage, temperature, and current), save them to a database, and raise an alarm if the voltage drops below a certain threshold.

The company has struggled with "Big Ball of Mud" architectures in the past, where features were scattered across global technical layers like `controllers`, `services`, and `repositories`. This made it difficult to understand what the system actually *does* just by looking at the folders.

Your task is to set up the initial directory structure and boilerplate for this new "Battery Monitoring" capability. The goal is a "Screaming Architecture" where the code is organized around the business process, making it easy to replace or extract the entire feature if needed.

## Output Specification

Provide a directory structure and the initial code (in TypeScript or the language of your choice) for this capability. This should include:
- The overall folder structure for the feature.
- The core service that applies the business rules for battery health.
- Necessary abstractions for saving readings and raising alarms.
- A technical implementation for those abstractions.
- A clear public API or entry point for the capability.

Focus on creating a structure that is cohesive and keeps all parts of the feature together while maintaining a clear separation between logic and technical details.
