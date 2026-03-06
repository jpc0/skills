# Testing for Refactoring Safety

## Problem/Feature Description

We are building a `Firmware Dispatcher` feature. Its job is to find the latest firmware versions and send them to IoT devices. The feature is split into multiple internal classes: a `VersionScanner`, a `MessageBuilder`, and a `DispatchService`. It also needs to talk to a database to log attempts and an external gateway to send the actual messages.

In the past, our team wrote "solitary" unit tests that mocked every internal helper class. This led to "brittle" tests that would break every time we tried to refactor our internal classes, even if the feature's behavior stayed the same. It also gave us very low confidence that the components actually worked together correctly.

Your task is to write a unit test for this `Firmware Dispatcher`. The goal is a test that provides high confidence and is safe to refactor. Specifically, the test should be able to pass even if the internal classes are split or merged, as long as the feature's core behavior (correctly logging and sending firmware) is maintained.

## Input Files

The following file is provided as a starting point:

=============== FILE: src/FirmwareDispatcher/FirmwareDispatcher.ts ===============
export class FirmwareDispatcher {
  constructor(private repo: IFirmwareRepository, private gateway: IIoTGateway) {}

  async dispatch(version: string, deviceId: string) {
    const log = await this.repo.createLog(version, deviceId);
    await this.gateway.send(version, deviceId);
    return log.id;
  }
}

export interface IFirmwareRepository {
  createLog(version: string, deviceId: string): Promise<any>;
}

export interface IIoTGateway {
  send(version: string, deviceId: string): Promise<void>;
}

## Output Specification

- Provide the implementation for a unit test suite for this feature.
- Show how the test provides high confidence by exercising the feature's logic as a whole.
- Use a technique that provides a stable implementation for external dependencies (like the database and gateway) without relying on automated mocking frameworks like Jest/Mockito.
- Demonstrate a test that is focused on behavior and state rather than implementation details.
- Provide any necessary code for the dependencies and the test runner.
