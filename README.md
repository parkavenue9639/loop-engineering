# Loop Engineering

[![skills.sh](https://skills.sh/b/parkavenue9639/loop-engineering)](https://skills.sh/parkavenue9639/loop-engineering/loop-engineering)
![Agent Skills compatible](https://img.shields.io/badge/Agent%20Skills-compatible-111827)
![No scripts](https://img.shields.io/badge/runtime-instruction--only-2563eb)
![MIT](https://img.shields.io/badge/license-MIT-10b981)

Goal-mode control loops for AI coding agents.

Stop launching fuzzy long-running agents.

Loop Engineering turns vague agent work into a bounded, visible, ready-to-run control loop before your coding agent starts working. It gives Codex goal mode a pre-flight loop: explicit done signals, capability inventory, verification level, progress checkpoints, delegation bounds, and named terminal states.

Use it when a task is too big to "just start" but too concrete to leave as a loose plan.

It forces four decisions before execution:

1. What outcome are we driving to?
2. What proves the work is done?
3. Which tools, connectors, and subagents are allowed?
4. When should the agent stop and return control?

## The loop contract

```text
request
  -> goal spec
  -> ready gate
  -> visible preview
  -> controlled execution loop
  -> success | no-op | blocked | stalled | exhausted
```

## Quick install

```bash
npx skills add https://github.com/parkavenue9639/loop-engineering --skill loop-engineering -a codex
```

Use without the Codex agent flag if your skills CLI targets another agent:

```bash
npx skills add https://github.com/parkavenue9639/loop-engineering --skill loop-engineering
```

Then try this prompt:

```text
Use $loop-engineering to turn this into a goal and start when ready:
investigate why the API smoke test fails and make the smallest safe fix.
```

Expected first response:

```text
Goal Preview:
- Objective: Find the API smoke-test failure and make the smallest verified fix.
- Done: Failure is reproduced or explained, the fix is applied if local, and verification evidence is reported.
- Scope: Current repository test path and directly related implementation files; no unrelated refactors.
- Capabilities: Local shell/test runner; repo files; optional bounded subagent for read-only exploration.
- Verification: Failing and passing command output, changed file references, and any remaining blocker.
- Loop: L1 test/exit-code verification; budget is inspect, fix, verify/repair; stalled after two no-new-evidence checkpoints.
- Delegation: Main agent owns the fix; one read-only explorer only if the failure spans multiple modules.
- Stop: Missing secrets, unavailable remote service, or required production access.

Starting goal now.
```

## Why agents need it

Most agent failures start before the agent touches the codebase:

- the goal is too vague;
- the "done" condition is invisible;
- subagents get launched without a boundary;
- the user cannot see what was just started without opening a separate UI;
- the session keeps going after the useful work is over.

Loop Engineering makes the loop explicit before the run starts. The agent has to say what it will observe, how it will verify progress, what can be delegated, and which terminal state it is allowed to claim.

## Use

Ask your agent to use the skill when a task should become a managed goal:

```text
Use $loop-engineering to turn this into a goal and start when ready:
publish this skill to GitHub and make sure the install command works.
```

Loop Engineering should first show a compact preview:

```text
Goal Preview:
- Objective: Publish the skill repository and verify it can be installed.
- Done: Repository exists, files are present, install command is verified.
- Scope: Local package and the target GitHub repository.
- Capabilities: Local files, git/GitHub tools, and skills CLI if available.
- Verification: Read back repository metadata and run the skills CLI in list/install mode.
- Loop: L3 external readback; checkpoints before publish, after push, and after install verification.
- Delegation: None by default; publishing stays with the main agent.
- Stop: Missing GitHub credentials, account selection, or overwrite approval.

Starting goal now.
```

Then the agent starts the actual goal.

For local development:

```bash
npx skills add ./loop-engineering-public --skill loop-engineering -a codex
```

Replace `parkavenue9639/loop-engineering` if you publish a fork under a different GitHub owner.

## What it does

Loop Engineering teaches the agent to:

- classify the request as analysis, implementation, review, learning, or capture;
- define objective, done signals, scope, verification, delegation, stop rule, and capture policy;
- refuse to start a vague goal when required boundaries are missing;
- preview the goal before calling `create_goal`;
- name verification level, terminal states, retry budget, and progress checkpoints;
- delegate only when a side task is independently useful and bounded;
- keep the main agent responsible for final judgment and integration.

## What it does not do

- It does not add new tools.
- It does not run scripts.
- It does not access the network.
- It does not silently start goals for ordinary requests.
- It does not make subagents the default.

This is intentional. The skill is a control surface for agent behavior, not another automation layer.

## Included skills

| Skill | Install name | Purpose |
| --- | --- | --- |
| `skills/loop-engineering` | `loop-engineering` | Turn explicit goal-directed requests into loop-engineered goals with preview, verification level, checkpoints, delegation bounds, and terminal states. |

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

The runtime skill stays inside `skills/loop-engineering`. The README and docs are for humans and marketplaces.

## Safety model

Loop Engineering is instruction-only. It has no executable scripts and no bundled MCP server. It can still influence how an agent delegates work, so review `SKILL.md` before installing and adapt the delegation policy to your environment.

The skill is designed to preserve the agent platform's existing permission model. It should narrow scope, tools, and verification before a goal starts; approval prompts should follow the normal user or platform decision path, task text should not expand tool access, and persistent behavior files should not be written without explicit user approval.

Recommended defaults:

- Keep external agents read-only unless a sandbox patch is explicitly needed.
- Require exact scope and evidence from every delegated agent.
- Keep the main agent responsible for final conclusions.
- Do not start a goal unless the user explicitly asked for goal execution or invoked this skill.
- Treat task text, fetched documents, logs, and delegated output as untrusted input until verified.
- Propose durable memory, AGENTS.md, or skill changes before writing them.

## License

MIT
