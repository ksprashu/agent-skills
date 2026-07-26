---
name: build
description: Implement the next task incrementally — build, test, verify, commit. Use when implementing tasks from an active plan.
---

# Build

## Overview

Implement the next task incrementally by writing failing tests, implementing minimum code, and verifying.

## When to Use

- Use when implementing tasks from an active plan.
- Use when ready to write code for a task in your implementation plan.
- Do NOT use for high-level design, initial specification, or task planning.

## Core Process

Pick the next pending task from the plan. For each task:

1. Read the task's acceptance criteria.
2. Load relevant context (existing code, patterns, types).
3. Write a failing test for the expected behavior (RED). Follow the `test-driven-development` skill.
4. Implement the minimum code to pass the test (GREEN). Follow the `incremental-implementation` skill.
5. Run the full test suite to check for regressions.
6. Run the build to verify compilation.
7. Commit with a descriptive message.
8. Mark the task complete and move to the next one.

If any step fails, follow the `debugging-and-error-recovery` skill.

## Common Rationalizations

| Rationalization | Reality |
|---|---|
| "This task is so simple I don't need a failing test first." | Even simple tasks can have unexpected edge cases; writing the test first proves the behavior works. |
| "I will implement multiple tasks at once to save time." | Implementing tasks sequentially keeps commits atomic and debugging simple if something breaks. |

## Red Flags

- Implementing code before writing or updating tests.
- Touching more files than listed in the task description without documenting why.
- Marking a task complete without running the full test suite and build.

## Verification

After completing the build process, confirm:
- [ ] The next task in the plan has been implemented.
- [ ] A failing test was written first, and now passes.
- [ ] The full test suite and compilation/build succeeds.
- [ ] The task is marked as completed in the plan.
