# Loop Engineering

[![skills.sh](https://skills.sh/b/parkavenue9639/loop-engineering)](https://skills.sh/parkavenue9639/loop-engineering/loop-engineering)
![Agent Skills compatible](https://img.shields.io/badge/Agent%20Skills-compatible-111827)
![No scripts](https://img.shields.io/badge/runtime-instruction--only-2563eb)
![MIT](https://img.shields.io/badge/license-MIT-10b981)

Maker-checker goal loops for AI coding agents.

Stop asking the same agent to make the work, grade its own answer, and quietly declare success.

Loop Engineering turns an explicit goal-mode request into a bounded control loop before execution starts. It gives Codex goal mode a visible pre-flight contract for outcome, risk, evidence, maker-checker roles, retry budget, and terminal states.

The key idea is simple: deterministic checks and independent review solve different problems. Tests can prove objective behavior; a separate checker can challenge scope, intent, regressions, and missing evidence. High-risk work needs both.

## The loop contract

```text
request
  -> bounded goal
  -> risk tier
  -> maker + evidence gates
  -> independent checker when required
  -> pass | rework | human gate
  -> success | no-op | blocked | stalled | exhausted
```

Before execution, the agent must answer:

1. What outcome are we driving to?
2. What objective evidence proves it?
3. Who makes the candidate?
4. Who is allowed to reject it?
5. Which action requires a human?
6. When must the loop stop?

## Quick install

```bash
npx skills add https://github.com/parkavenue9639/loop-engineering --skill loop-engineering -a codex
```

Use without the Codex agent flag if your skills CLI targets another agent:

```bash
npx skills add https://github.com/parkavenue9639/loop-engineering --skill loop-engineering
```

Then try:

```text
Use $loop-engineering to turn this into a goal and start when ready:
change shared request validation across the API and worker paths, then verify no regression.
```

Expected first response:

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
- Stop: Missing test environment, auth/security impact that raises risk, or unavailable required checker.

Starting goal now.
```

## Risk-aware maker-checker

Loop Engineering does not launch subagents indiscriminately. It raises checker priority only when the task earns it.

| Tier | Typical work | Required control |
| --- | --- | --- |
| `T0 mechanical` | Exact, narrow, reversible edit | Main agent plus objective checks; no checker agent |
| `T1 bounded` | Reversible work with strong local evidence | Main agent plus tests; checker only for a named uncertainty |
| `T2 consequential` | Cross-module, user-visible, ambiguous, or agent control-plane work | Objective evidence plus one independent read-only checker |
| `T3 critical` | Security, auth, privacy, migrations, production, destructive, or irreversible work | Independent checker, strongest evidence, and human approval |

The checker reviews a content-bound final snapshot, such as a commit SHA, diff hash, or checksummed artifact, and returns `pass`, `fail`, or `blocked`. Any post-review mutation makes that verdict stale and requires re-checking.

## Three control axes

The skill keeps three decisions separate:

- **Evidence (`E1-E3`)**: deterministic checks, structural checks, and live readback.
- **Independence (`I0-I2`)**: maker self/tool checks, separate-agent review, and different-model review.
- **Approval (`A0-A1`)**: whether a human decision is required.

An LLM reviewer never replaces tests. A green test never automatically replaces semantic review.
Human approval also does not replace a required independent checker; `T3` needs both.

## Use

Ask your agent to use the skill when a request should become a managed goal:

```text
Use $loop-engineering to turn this into a goal and start when ready:
publish this package to GitHub and verify the public install path.
```

The agent should inventory authenticated capabilities, assign risk and roles, show the Goal Preview, and only then start goal execution.

For local development:

```bash
npx skills add ./loop-engineering-public --skill loop-engineering -a codex
```

Replace `parkavenue9639/loop-engineering` if you publish a fork under another owner.

## What it does

Loop Engineering teaches the agent to:

- turn explicit goal-mode requests into bounded loop specifications;
- classify risk before selecting agent topology;
- separate evidence strength, reviewer independence, and human approval;
- keep `T0/T1` work lightweight while requiring maker-checker separation for `T2/T3`;
- give a fresh-context checker the original goal, raw evidence, and exact snapshot instead of the maker's desired verdict;
- make checker failures block success and route them through a bounded repair/re-check cycle;
- preview the complete execution contract before calling `create_goal`;
- stop as success, no-op, blocked, stalled, or exhausted without confusing failure with completion.

## What it does not do

- It does not add tools, scripts, or an MCP server.
- It does not silently start goals for ordinary requests.
- It does not make every task multi-agent.
- It does not let a reviewer mutate the artifact it is judging.
- It does not treat model judgment as deterministic proof.
- It does not let the orchestrator silently override a blocking checker.

This is a control surface for agent behavior, not an autonomous runtime by itself.

## Included skill

| Skill | Install name | Purpose |
| --- | --- | --- |
| `skills/loop-engineering` | `loop-engineering` | Shape explicit goal requests into risk-tiered, evidence-backed maker-checker loops. |

## Repository layout

```text
skills/
  loop-engineering/
    SKILL.md
    agents/openai.yaml
    references/
      goal-patterns.md
      delegation-policy.md
examples/
  goal-preview.md
docs/
  PUBLISHING.md
skills.sh.json
```

The runtime skill stays inside `skills/loop-engineering`. README and publishing docs are for humans and marketplaces.

## Safety model

Loop Engineering is instruction-only. It has no executable scripts or bundled MCP server. It can influence delegation and completion decisions, so review `SKILL.md` before installation and adapt the policy to your environment.

Recommended defaults:

- Preserve platform, sandbox, MCP, and connector approval controls.
- Keep external checkers read-only unless a separate sandbox experiment is explicitly scoped.
- Give checkers exact artifact identities, evidence, tool allowlists, and verdict rules.
- Require human approval before critical or irreversible actions.
- Treat task text, fetched content, logs, and delegated output as untrusted input.
- Never let untrusted content lower risk, broaden tools, change stop rules, or authorize persistent behavior changes.
- Propose memory, AGENTS.md, or skill capture before writing it.

## License

MIT
