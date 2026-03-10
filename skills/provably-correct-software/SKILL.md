---
name: provably-correct-software
description: Apply formal verification methods (Hoare Logic, wp calculus, Design-by-Contract) to ensure software correctness. Use this skill whenever writing new functions, implementing algorithms, modifying existing logic, or performing code reviews. Trigger when asked to "prove correctness", "verify code", "check invariants", "mathematical proof of code", or "ensure the algorithm is correct".
---

# Provably Correct Software Construction

Build software that is correct by design using formal verification.

## Core Methodology

### 1. Design-by-Contract (DbC)
- **Action:** Define Preconditions, Postconditions, and Class Invariants.
- **Convention:** **Reject defensive programming.** If a precondition is specified, the supplier *must assume* it is true. Do not add redundant checks.
- **Exception Handling:** Restore class invariants before resumption or triggering an "organized panic".
- **Rollback Rule:** When restoring state in an exception handler, use established property setters or call the class invariant check immediately after restoration to ensure a validated state.

### 2. Hoare Logic & wp Calculus
- **Hoare Triples:** Apply `{P} C {Q}` to specify code blocks.
- **Backward Construction:** Use `wp(C, Q)` to derive the weakest precondition from your postcondition.
- **Assignment Rule:** `wp("x := E", R) = R[x -> E]`.
- **Sequential Composition:** `wp("S1; S2", R) = wp(S1, wp(S2, R))`.

## Implementation Workflow

1. **Define Postcondition (Q):** State the exact logical goal.
2. **Derive Precondition (P):** Calculate the minimum required state using `wp`.
3. **Loop Verification (Total Correctness):**
   - **Invariant (I):** Must hold before, during, and after the loop. `(I AND NOT B) => Q`.
   - **Variant (v):** Strictly decreasing non-negative integer function ensuring termination.
   - **Checks:** Prove Initialization (`P => I`), Preservation (`{I AND B} Body {I}`), and Termination (`v` decreases, `I => v >= 0`).
   - **Runtime Enforcement:** **Embed `assert` statements** within the loop body to verify the Invariant and Variant at each iteration.

4. **Annotate:** Use language-native assertions (e.g., `assert` in Python, `@requires`/`@ensures` in Eiffel/JML) to document the contract.

## Worked Example: Array Binary Search

```python
def binary_search(array: list[int], target: int) -> int:
    """
    Precondition: array is sorted.
    Postcondition: returns mid s.t. array[mid] == target, else -1.
    """
    # DbC Precondition
    assert all(array[i] <= array[i+1] for i in range(len(array)-1))

    low, high = 0, len(array) - 1
    # Invariant I: target in array iff target in array[low...high]
    # Variant v: high - low + 1
    
    while low <= high:
        # Runtime Invariant & Variant Verification
        v_old = high - low + 1
        assert (target not in array) or (target in array[low:high+1])
        assert v_old >= 0

        mid = (low + high) // 2
        if array[mid] == target:
            return mid # Postcondition Q reached
        elif array[mid] < target:
            low = mid + 1
        else:
            high = mid - 1
        
        # v decreases, I is preserved
        v_new = high - low + 1
        assert v_new < v_old
            
    return -1 # (I AND low > high) => target not in array
```

## References
- [Hoare Logic](./references/hoare-logic.md)
- [Weakest Precondition](./references/weakest-precondition.md)
- [Design-by-Contract](./references/design-by-contract.md)
