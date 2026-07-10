# Goal Preview Example

User request:

```text
Use $loop-engineering to turn this into a goal and start when ready:
change shared request validation across the API and worker paths, then verify no regression.
```

Expected preview:

```text
Goal Preview:
- Objective: Update shared request validation across both paths and prove behavior remains compatible.
- Done: Both callers use the intended rule, focused tests pass, and the final diff passes independent review.
- Scope: Shared validator, API/worker adapters, and directly related tests; no unrelated refactors.
- Risk: T2 because the change crosses modules and affects user-visible request handling.
- Capabilities: Repository files, test runner, and one fresh-context read-only checker.
- Roles: Main agent orchestrates and makes; fresh internal agent checks; no human gate unless auth or production impact appears.
- Evidence: E1 focused tests plus E2 diff/scope checks; checker fails on unmet behavior, regressions, or scope drift.
- Loop: Inspect, patch, test, check, then one repair/re-check cycle.
- Stop: Missing test environment, risk escalation without the required gate, or unavailable checker.

Starting goal now.
```

Expected created goal body:

```text
Objective:
Update shared request validation across the API and worker paths and prove the final behavior remains compatible.

Done Signals:
- Existing validation behavior and both call paths are identified before editing.
- The intended validation rule is applied consistently without unrelated refactors.
- Focused tests for API and worker behavior pass, or unavailable coverage is explicit.
- One independent read-only checker returns `pass` for the exact final diff.
- Changed files, evidence, and residual risk are reported.

Scope:
The shared validator, API and worker adapters that call it, and directly related tests. Exclude unrelated request handling, auth policy, and production configuration.

Risk Tier:
`T2 consequential` because the change crosses modules and affects user-visible request handling. Raise to `T3` if inspection reveals auth, privacy, production, or destructive impact. Do not lower risk within this run.

Capability Inventory:
Use repository files, local shell commands, the focused test runner, and one fresh-context internal checker. Do not require remote services unless existing tests depend on them.

Role Topology:
The main agent is orchestrator and maker. A separate fresh-context internal agent is the read-only checker and must not modify the reviewed files. No human gate is required at T2; require one if risk rises to T3.

Evidence Plan:
Use `E1 deterministic` focused tests plus `E2 structural` diff, scope, and call-path checks. Record a content-bound final diff hash or commit SHA plus repository state for checker review.

Checker Contract:
Give the checker the original objective, Done Signals, scope, T2 trigger, exact final diff, and raw test output. It returns `pass`, `fail`, or `blocked`. Fail on unmet behavior, regressions, scope drift, stale evidence, or a mismatch between the reviewed and final snapshot. Any maker edit invalidates the verdict.

Loop Control:
Inspect baseline, make the smallest change, run evidence, run checker, and allow one repair/re-check cycle. Checkpoint after baseline, maker output, and checker verdict. Terminal states: success, no-op, blocked, stalled, exhausted.

Stop Rule:
Stop if the test environment is unavailable without an honest fallback, risk rises without the required checker or human gate, or two checkpoints add no new evidence.

Capture Policy:
Propose durable capture only if the task reveals a reusable validation or maker-checker rule.
```
