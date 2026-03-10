1. The Core Concept

The weakest pre-condition, denoted as wp(S,R), characterizes the set of all initial states such that activating a mechanism (or statement) S will certainly result in a properly terminating execution that leaves the system in a final state satisfying the desired post-condition R.

In practice, this means programming is a goal-directed activity: you start by precisely defining your desired outcome (R) and work backwards through your statements (S) to find the starting conditions required to make that outcome a reality.
2. Fundamental Properties

Predicate transformers (the rules used to derive wp) follow four specific properties that keep the logic sound:

    The Law of the Excluded Miracle: wp(S,F)=F. You cannot find an initial state that guarantees a mechanism will establish a false condition.

    Monotonicity: If Q⇒R for all states, then wp(S,Q)⇒wp(S,R) for all states.

    Conjunction: wp(S,Q) and wp(S,R)=wp(S,Q and R).

    Disjunction: wp(S,Q) or wp(S,R)⇒wp(S,Q or R). For fully deterministic mechanisms, this implication becomes an equality.

3. The Calculus of Basic Statements

When writing sequential code, you evaluate how each statement transforms the state by substituting your goal into these formulas:

    The Empty Statement (skip): wp("skip",R)=R. Doing nothing requires the goal to already be true.

    The Failing Statement (abort): wp("abort",R)=F. Attempting to activate an abort statement will fail to reach a final state, meaning no initial state can guarantee success.

    The Assignment Statement (x := E): wp("x := E",R)=RE→x​. To find the weakest pre-condition, you take your post-condition R and replace every occurrence of the variable x with the expression E.

        Example: If your goal is a=7, then wp("a := 7",a=7)=(7=7)=T. This means any initial state guarantees the goal.

4. Structuring Control Flow

    Sequential Composition (S1; S2): wp("S1; S2",R)=wp(S1,wp(S2,R)). You work completely backwards. Supply the final goal R to the second statement to get an intermediate condition, and supply that intermediate condition to the first statement.

    Alternative Construct (if...fi): For an if statement to guarantee R, two things must be true. First, at least one of its conditions (guards) must be true to prevent the program from aborting. Second, for every guard that evaluates to true, executing its corresponding statement block must guarantee R.

5. Practical Implementation: Mastering Loops

Calculating the exact weakest pre-condition for a loop (do...od) requires complex recurrence relations calculating termination after k steps. However, Dijkstra provides a highly practical heuristic for software implementation: The Fundamental Invariance Theorem for Loops.

When implementing a loop, you should explicitly design it around these elements:

    The Invariant (P): A generalized version of your final goal that is easy to establish before the loop starts, and is kept true after every single iteration.

    The Guards (BB): The condition(s) under which the loop continues.

    The Post-Condition (R): The loop terminates when all guards are false (non BB). Therefore, you must design your invariant so that (P and non BB)⇒R.

    Proof of Termination: To ensure the loop does not run infinitely, define a monotonically decreasing integer function of the state, t. Ensure that your invariant guarantees t≥0 , and that every execution of a guarded command decreases t by at least 1.

A Note on Liberal Pre-conditions

If you are writing a system where infinite loops are permissible or out of your control, but you strictly want to avoid producing incorrect outputs, you can look into "weakest liberal pre-conditions" (wlp). A liberal pre-condition guarantees that the system won't produce the wrong result, but it leaves non-termination as an alternative.
