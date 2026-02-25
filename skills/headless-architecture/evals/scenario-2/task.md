# Cross-platform Chess Engine

## Problem/Feature Description

We are building a chess game. Our vision is to have a single engine that can be compiled to WASM for web browsers, used natively on mobile (iOS/Android), and even run in a CLI. 

The engine should handle the board state, manage turns, and validate whether a move is legal. For example, it must know that a pawn can move forward one space (or two on its first move) and that a king cannot move into check.

To support our cross-platform goals, the engine must be platform-agnostic, with no dependencies on any UI frameworks or operating system-specific APIs. We need a clean, explicit boundary between the chess logic and whatever UI (web, mobile, or CLI) happens to be using it.

Please implement the initial chess engine logic and the interface for the UI.

## Output Specification

The output should include:
1.  A platform-agnostic core module that handles chess rules, board state, and move validation.
2.  An explicit bridge or API contract (e.g., FFI-compatible or WASM-ready) for UI integration.
3.  A placeholder or example UI component that shows how it would interact with the engine.
4.  Organize your files into a logical directory structure that separates the business logic from the presentation.
