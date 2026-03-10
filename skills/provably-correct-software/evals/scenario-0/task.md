# Coordinate System Transformation

## Problem Description

You are tasked with implementing a critical part of a high-precision geometry engine. Your goal is to transform a 2D point from a local coordinate system to a global one.

The output must satisfy the following goal relative to the input `(x, y)`:
- The global `X` coordinate must be exactly `2*x + 10`.
- The global `Y` coordinate must be exactly `3*y - 5`.

You must implement a function `transform_coordinates(x, y)` that achieves this state.

**Requirements:**
- To ensure traceability and precision, do NOT calculate the final values in a single expression.
- Use a sequence of distinct assignments to calculate the final `X` and `Y`.
- You must include a rigorous derivation in the code's comments to prove that the sequence of assignments you've chosen logically guarantees the required output for all valid inputs.

## Output Specification

Produce a Python file `geometry.py` containing the `transform_coordinates` function. The function must be robust and include the step-by-step logical justification in the comments.
