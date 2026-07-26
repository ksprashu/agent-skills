---
name: code-simplify
description: Simplify code for clarity and maintainability — reduce complexity without changing behavior. Use when code is hard to read or has accumulated complexity.
---

# Code Simplify

## Overview

Simplify recently changed or target code for clarity and maintainability, reducing complexity without changing external behavior.

## When to Use

- Use when code is hard to read, maintain, or has accumulated unnecessary complexity.
- Use when reviewing recently modified code to see if it can be written more cleanly.
- Do NOT use when you want to change the behavior, fix a bug, or add new features.

## Core Process

Simplify recently changed code (or the specified scope) while preserving exact behavior:

1. Read GEMINI.md and study project conventions.
2. Identify the target code — recent changes unless a broader scope is specified.
3. Understand the code's purpose, callers, edge cases, and test coverage before touching it.
4. Scan for simplification opportunities:
   - Deep nesting → guard clauses or extracted helpers
   - Long functions → split by responsibility
   - Nested ternaries → if/else or switch
   - Generic names → descriptive names
   - Duplicated logic → shared functions
   - Dead code → remove after confirming
5. Apply each simplification incrementally — run tests after each change.
6. Verify all tests pass, the build succeeds, and the diff is clean.

If tests fail after a simplification, revert that change and reconsider. Use `code-review-and-quality` to review the result.

## Common Rationalizations

| Rationalization | Reality |
|---|---|
| "I can simplify this code without running tests because the change is trivial." | Even trivial refactorings can introduce subtle bugs; always run the test suite to verify. |
| "I should rewrite this entire module to make it cleaner." | Complete rewrites are high-risk. Incremental, targeted simplifications are much safer and easier to verify. |

## Red Flags

- Modifying code behavior, inputs, or outputs while attempting to simplify.
- Refactoring code that has no existing test coverage.
- Accumulating a large set of unverified changes before running the test suite.

## Verification

After completing the code simplification, confirm:
- [ ] Code complexity is significantly reduced (fewer lines, less nesting, clearer names).
- [ ] No behavioral changes were introduced.
- [ ] All unit and integration tests pass successfully.
- [ ] The compiler/builder completes without warning or errors.
