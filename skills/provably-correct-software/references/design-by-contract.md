Bertrand Meyer's Design by Contract (DbC) is a fantastic paradigm for building reliable software, and it shifts the way you think about writing code. Here is a practical summary of how to apply its principles directly to your implementation process.
1. Define Explicit Contracts with Assertions

To implement DbC, you must treat software construction as a series of documented contracting decisions between a "client" (the calling code) and a "contractor" (the routine being called).

Every routine should ideally have a contract that protects both sides:

    Preconditions: Requirements that the client must satisfy before calling the routine. If the precondition is not met, the routine is not bound to do anything.

    Postconditions: Properties that the routine (contractor) guarantees will be true upon completion, provided the preconditions were met.

2. Establish Class Invariants

Beyond individual routines, you must define overarching rules for your data structures, known as class invariants.

    An invariant is a property that must be satisfied by all instances of a class.

    It must be true immediately after an object is created (ensured by the creation procedure).

    It must be preserved by every publicly available (exported) routine: if it was true on entry, the routine guarantees it will be true on exit.

3. Stop Defensive Programming

One of the most actionable—and sometimes controversial—takeaways from DbC is the rejection of "defensive programming".

    Do not litter your code with redundant checks (e.g., checking for the same error in both the client and the supplier).

    Blind, redundant error checking inevitably increases software complexity, which leads to a decrease in reliability.

    Instead, explicitly assign responsibility: either a consistency condition is part of the precondition (and must be guaranteed by the client), or it is not (and the supplier must handle it).

4. Apply Disciplined Exception Handling

In DbC, an "exception" occurs when a subtask fails during execution. If a routine is unable to satisfy its contract, that is considered a "failure". When an exception occurs, you only have two valid strategies:

    Resumption: Put the objects back into a stable state (which means restoring the class invariant) and make a new attempt, either with the same strategy or an alternative one.

    Organized Panic: Put the objects back into a stable state, give up on the contract entirely, and report the failure to the caller by triggering an exception.

5. Follow the Rules of Subcontracting (Inheritance)

When you use inheritance and dynamic binding to redefine a routine in a subclass, you are essentially "subcontracting" the work. An honest subcontractor must play by strict rules so they don't break the original client's expectations:

    You may weaken the precondition, meaning your new routine can accept cases that the original contractor would have rejected.

    You may strengthen the postcondition, meaning your new routine can return a better or more specific result than initially agreed upon.

    You must never place higher demands on the client, nor can you return less than what the original contract promised.
