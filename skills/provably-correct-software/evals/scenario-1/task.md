# Data Stream Consistency Analysis

## Problem Description

Your team is developing a high-reliability data processing engine. You need a function that analyzes a sequence of integer sensor readings to identify the longest continuous streak of identical values. For example, in the dataset `[1, 2, 2, 2, 3]`, the longest streak of identical values has a length of 3.

Implement a function `max_streak(data)` that calculates this length.

**Requirements:**
- The function must use a loop to process the data sequence.
- You must provide a formal mathematical argument in the comments that the loop is logically guaranteed to return the correct maximum length for any input array.
- You must also provide a rigorous guarantee in the comments that the loop will terminate for any finite input.
- The code itself should include mechanisms to continuously verify its internal state during execution to ensure the mathematical argument holds.

## Output Specification

Produce a Python file `streaks.py` containing the `max_streak` function. The code must be self-verifying and include the formal correctness and termination arguments in the comments.
