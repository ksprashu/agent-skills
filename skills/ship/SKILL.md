---
name: ship
description: Run the pre-launch checklist via parallel fan-out to specialist personas, then synthesize a go/no-go decision. Use when preparing to deploy or launch the application.
---

# Ship

## Overview

Run the pre-launch checklist via parallel fan-out to specialist personas (`code-reviewer`, `security-auditor`, and `test-engineer`), synthesize findings, and produce a unified go/no-go decision with a rollback plan.

## When to Use

- Use when preparing to deploy, merge, or launch a production-bound change.
- Use when the blast radius of changes is non-trivial.
- Do NOT use for small, low-risk changes (2 files or fewer, under 50 lines, and no security/payments impact).

## Core Process

`ship` is a **fan-out orchestrator**. It runs three specialist personas in parallel against the current change, then merges their reports into a single go/no-go decision with a rollback plan. The personas operate independently — no shared state, no ordering.

### Phase A — Parallel fan-out

Spawn three subagents concurrently. Custom subagents in `agents/` are used:

1. **`code-reviewer`** — Run a five-axis review (correctness, readability, architecture, security, performance) on the staged changes or recent commits. Output the standard review template.
2. **`security-auditor`** — Run a vulnerability and threat-model pass. Check OWASP Top 10, secrets handling, auth/authz, dependency CVEs. Output the standard audit report.
3. **`test-engineer`** — Analyze test coverage for the change. Identify gaps in happy path, edge cases, error paths, and concurrency scenarios. Output the standard coverage analysis.

*Note: Issue all three subagent tool calls in a single assistant turn so they execute in parallel. If subagents are unavailable, invoke each persona's system prompt sequentially in the main context.*

### Phase B — Merge in main context

Once all three reports are back, synthesize them:

1. **Code Quality** — Aggregate Critical/Important findings from `code-reviewer` and any failing tests, lint, or build output. Resolve duplicates between reviewers.
2. **Security** — Promote any Critical/High `security-auditor` findings to launch blockers. Cross-reference with `code-reviewer`'s security axis.
3. **Performance** — Pull from `code-reviewer`'s performance axis; cross-check Core Web Vitals if applicable.
4. **Accessibility** — Verify keyboard nav, screen reader support, contrast.
5. **Infrastructure** — Env vars, migrations, monitoring, feature flags. Verify directly.
6. **Documentation** — README, ADRs, changelog. Verify directly.

### Phase C — Decision and rollback

Produce a single output using the following markdown template:

```markdown
## Ship Decision: GO | NO-GO

### Blockers (must fix before ship)
- [Source persona: Critical finding + file:line]

### Recommended fixes (should fix before ship)
- [Source persona: Important finding + file:line]

### Acknowledged risks (shipping anyway)
- [Risk + mitigation]

### Rollback plan
- Trigger conditions: [what signals would prompt rollback]
- Rollback procedure: [exact steps]
- Recovery time objective: [target]

### Specialist reports (full)
- [code-reviewer report]
- [security-auditor report]
- [test-engineer report]
```

## Rules

1. The three Phase A personas run in parallel — never sequentially.
2. Personas do not call each other. The main agent merges in Phase B.
3. The rollback plan is mandatory before any GO decision.
4. If any persona returns a Critical finding, the default verdict is NO-GO unless the user explicitly accepts the risk.
5. **Skip the fan-out only if all of the following are true:** the change touches 2 files or fewer, the diff is under 50 lines, and it does not touch auth, payments, data access, or config/env. Otherwise, default to fan-out.

## Common Rationalizations

| Rationalization | Reality |
|---|---|
| "I can skip the rollback plan because our deployment process is reliable." | Deployment processes can fail in unexpected ways. A documented rollback plan is mandatory for safety. |
| "The change is minor so I can run these reviews sequentially." | Parallel fan-out is more efficient and ensures independent perspective checks without cross-contamination. |

## Red Flags

- Proceeding with a "GO" decision while there are active unmitigated Critical blockers.
- Omitting the rollback plan or specifying vague rollback steps (e.g. "git revert").
- Letting personas interact with or delegate to each other.

## Verification

After completing the ship process, confirm:
- [ ] All three specialist reports have been obtained (or simulated sequentially if subagents are disabled).
- [ ] A synthesized go/no-go verdict has been determined.
- [ ] A mandatory, detailed rollback plan has been documented.
- [ ] The final synthesized report is presented to the user.
