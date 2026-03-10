What is Hoare Logic?

Hoare Logic is a deductive proof system used to formally specify what a program does and verify that it meets those specifications. While it was originally proposed for manual, direct program verification, doing so is tedious and error-prone for large programs. Today, it forms the foundation for computer-assisted verification tools.
The Core Notation: Hoare Triples

To specify behavior, you use a notation called a Hoare triple, written as {P}C{Q}.

    C: Represents the command or code snippet.

    P (Precondition): A condition on the program variables that must be true before C executes.

    Q (Postcondition): A condition that must hold true after C finishes executing.

Partial vs. Total Correctness

When verifying software, you must decide which level of correctness you are aiming for:

    Partial Correctness ({P}C{Q}): This specification asserts that if the program C starts in a state satisfying P, and if the execution of C terminates, the final state will satisfy Q. It does not guarantee that the program will actually finish running.

    Total Correctness ([P]C[Q]): This is a stronger specification asserting that if C starts in state P, the execution will terminate, and afterward, Q will hold. Informally, total correctness equals partial correctness plus termination.

How to Use Hoare Logic in Software Implementation

If you are using modern verification tools based on Hoare Logic, the workflow generally follows an "annotate and generate" architecture.

1. Annotate Your Code
Before starting a proof, you must embed logical statements (assertions) within your code to express the conditions expected at intermediate points.

    Finding Invariants: For WHILE loops, you must provide an invariant. An invariant is a property that holds true initially, is preserved unchanged during each iteration of the loop body, and, when combined with the termination condition, yields the desired result.

    Adding Variants (for Total Correctness): To prove that a loop terminates, you must also define a "variant". This is a non-negative quantity that decreases on every single iteration of the loop.

2. Generate Verification Conditions (VCs)
Once your specifications and code are properly annotated, a VC generator mechanically transforms them into a set of standard mathematical and logical statements called Verification Conditions. This process effectively "compiles away" the programming constructs, leaving only arithmetic and logic.

3. Prove the Verification Conditions
The final step is to prove these generated mathematical statements, which is typically handled by automated theorem provers. If all the generated VCs can be proven, the original program specification {P}C{Q} is formally verified as correct.
Alternative Approach: Constructing Code

Instead of writing code first and verifying it later, you can use Edsger W. Dijkstra's theory of Weakest Preconditions to construct the code.

    The weakest precondition wp(C,Q) calculates the weakest starting state required to guarantee that command C achieves the postcondition Q.

    This approach is used as a theory of refinement—starting with non-deterministic pseudo-code and calculating your way down to deterministic, efficient code that inherently satisfies the postcondition.
