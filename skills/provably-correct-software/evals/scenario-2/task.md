# High-Reliability Storage System

## Problem Description

You are building a storage component for a safety-critical system. You need to implement a class `DataStore` that manages a fixed-capacity collection of items.

The `DataStore` must provide two primary operations: `add(item)` and `retrieve()`.

**Requirements:**
- The storage system must be inherently robust and must never exceed its defined capacity.
- You must define clear responsibilities for both the caller and the storage class.
- The implementation must avoid redundant checks for conditions that the caller is expected to respect.
- You must include logic that consistently enforces all system constraints throughout the lifecycle of the `DataStore` object.
- All system properties and operational requirements must be clearly documented in the code.

## Output Specification

Produce a Python file `storage.py` containing the `DataStore` class. Ensure that the logic is robust and uses built-in verification mechanisms to enforce all system rules.
