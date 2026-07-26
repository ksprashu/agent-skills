---
name: spec
description: Start spec-driven development — write a structured specification before writing code. Use when starting a new project, feature, or significant change.
---

# Spec

## Overview

Initiate spec-driven development to establish a clear, structured specification of requirements, constraints, and architecture before writing code.

## When to Use

- Use when starting a new project, feature, or any significant implementation change.
- Use when requirements from the user are vague, ambiguous, or incomplete.
- Do NOT use for simple one-line fixes or trivial spelling/styling corrections.

## Core Process

Begin by understanding what the user wants to build. Ask clarifying questions about:

1. The objective and target users.
2. Core features and acceptance criteria.
3. Tech stack preferences and constraints.
4. Known boundaries (what to always do, ask first about, and never do).

Then generate a structured spec covering all six core areas: objective, commands, project structure, code style, testing strategy, and boundaries.

Save the spec as `SPEC.md` in the project root and confirm with the user before proceeding.

## Common Rationalizations

| Rationalization | Reality |
|---|---|
| "I can start writing code immediately and write the spec later." | Coding without a spec often leads to wrong assumptions, wasted effort, and architectural debt. |
| "The requirements are simple enough that a spec is overkill." | Even simple requirements have hidden assumptions. Establishing a SPEC.md aligns expectations perfectly. |

## Red Flags

- Creating SPEC.md without asking clarifying questions on ambiguous requirements.
- Omitting boundaries or testing strategy from the specification document.
- Starting the implementation phase before the human has reviewed and approved SPEC.md.

## Verification

After completing the spec process, confirm:
- [ ] Clarifying questions were asked and answered.
- [ ] A structured spec covering all six core areas is created.
- [ ] The specification is saved to `SPEC.md` in the project root.
- [ ] The human has reviewed and approved the spec.
