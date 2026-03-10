# Verified Greatest Common Denominator Logic

## Problem Description

The following algorithm calculates the Greatest Common Divisor (GCD) for two positive integers:

```python
def calculate_gcd(a, b):
    x = a
    y = b
    while x != y:
        if x > y:
            x = x - y
        else:
            y = y - x
    return x
```

**Requirements:**
- You must provide a comprehensive formal proof that this algorithm correctly calculates the GCD for all positive inputs.
- Your proof must also include a rigorous mathematical argument that the algorithm is guaranteed to terminate.
- Annotate the implementation with the formal logical steps that demonstrate its correctness throughout its execution.
- Your documentation should be structured to show exactly how the algorithm transforms its state toward the final result.

## Output Specification

Produce a Python file `gcd_verified.py` with the provided algorithm and the full formal verification documented in the comments.
