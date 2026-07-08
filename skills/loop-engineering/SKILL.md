---
name: loop-engineering
description: Apply loop-engineering controls to an explicitly goal-directed or skill-invoked request and turn it into a ready-to-run Codex goal. Use when the user asks to use goal mode, create/start/shape a goal, turn a normal request into an executable loop, decide whether a task is ready for goal execution, or design delegation policy for a goal using internal sub-agents or external_agent MCP. Do not silently start a goal for ordinary requests unless the user explicitly invokes goal mode or this skill for execution.
---

# Loop Engineering

## Purpose

Shape a user's request into a bounded Codex goal, decide whether it is ready to start, and start the goal directly when the user has explicitly authorized goal execution. Use loop-engineering controls to make verification, progress, delegation, and terminal states explicit before execution.

The skill is a pre-flight controller for goal mode. It does not complete the target task by itself; it creates the execution loop that goal mode will run.

## Core Contract

Use this skill only when the user explicitly mentions goal mode, asks to create/start/shape a goal, invokes `$loop-engineering`, or asks to turn a request into an executable loop. A normal complex task is not enough authority to call `create_goal`.

When authorized and ready, call `create_goal` with a compact objective that includes the goal, done signals, scope, verification, delegation policy, stop rule, and capture policy. Then execute the goal normally.

When not ready, do not start a vague goal. Ask the minimum question needed or state the assumptions that would make it ready.

## Goal-Shaping Loop

1. Classify the request:
   - `analysis`: read-only investigation, research, trace diagnosis, architecture review.
   - `implementation`: code, document, artifact, or workflow change.
   - `review`: independent risk scan, PR/MR review, design critique, validation.
   - `learning`: study, synthesis, best-practice extraction, skill or memory design.
   - `capture`: decide whether experience should become memory, AGENTS.md guidance, knowledge-vault content, or a new skill.
2. Draft the goal spec:
   - Objective: one sentence describing the durable outcome.
   - Done Signals: 2-5 observable conditions that prove completion.
   - Scope: paths, systems, repos, artifacts, permissions, and excluded areas.
   - Capability Inventory: available tools, MCP connectors, CLIs, credentials, local files, and external systems that can complete the task end to end.
   - Verification: commands, tests, traces, citations, screenshots, review checks, or evidence format.
   - Loop Control: verification level, progress checkpoint cadence, retry or no-progress budget, and named terminal states.
   - Delegation Policy: allowed agents, max concurrency, read/write boundaries, and evidence requirements.
   - Stop Rule: concrete reasons to return control instead of continuing.
   - Capture Policy: whether and where durable learning may be proposed.
3. Run the Ready-to-Start gate.
4. If ready and authorized, show a concise Goal Preview to the user.
5. Create the goal and execute it.
6. If not ready, return a short "Not ready" report with the missing fields and the next question.

See `references/goal-patterns.md` for reusable goal templates.

## Loop Control

Treat the goal as a bounded loop specification, not just a task description.

Set the strongest honest verification level available:

- `L1 deterministic`: command exit code, test, assertion, exact diff, checksum, or golden output.
- `L2 structural`: schema, linter, static rule, policy checklist, or required artifact shape.
- `L3 field truth`: CI result, deploy result, live service state, user-visible URL, external system readback, or delayed operational evidence.
- `L4 independent judgment`: separate model, reviewer, or subagent applies a rubric. Never let the maker approve its own work at this level.
- `L5 human checkpoint`: user decision or approval is required before success can be claimed.

Prefer `L1` or `L2` for autonomous goals. If only `L4` or `L5` is possible, say so in the goal and keep the loop assisted rather than pretending it is self-verifying.

Name terminal states explicitly:

- `success`: done signals met with the promised evidence.
- `no-op`: investigation shows no change is needed, with evidence.
- `blocked`: required input, permission, credential, system state, or external dependency is missing.
- `stalled`: repeated iterations produce no new evidence or useful state change.
- `exhausted`: the declared budget is consumed without meeting done signals.

Use a progress checkpoint whenever the goal needs multiple tool calls, retries, or delegated work. A checkpoint should answer:

- What did we observe?
- What changed since the previous checkpoint?
- Which done signal or blocker does this affect?
- What is the next action, and why is it not repeating the same failed move?

Default budget: one investigation pass, one implementation or synthesis pass, and one verification or repair pass. Raise the budget only when the goal says why more iterations are useful. If two consecutive checkpoints produce no new evidence, mark the goal `stalled` or ask for new input.

## Ready-to-Start Gate

Start the goal only when all conditions are true:

- The user explicitly authorized goal execution by naming goal mode, asking to start a goal, invoking this skill for execution, or saying to create a loop and start once ready.
- The objective can be written as one concrete sentence.
- Done signals are observable by local evidence, source citations, tests, artifacts, traces, logs, or user-confirmable output.
- Scope is bounded enough to avoid uncontrolled expansion.
- Tool, connector, credential, repository, and external-system capabilities needed for the first useful action are known or explicitly marked as unavailable.
- Verification level is named and possible, or the goal explicitly includes a known evidence gap and an assisted terminal state.
- Terminal states are named well enough that success, no-op, blocked, stalled, and exhausted cannot be confused.
- A progress checkpoint cadence and retry/no-progress budget exist for multi-step work.
- Delegation policy is clear, or delegation is explicitly disabled.
- No required input blocks the first useful action.
- No active unfinished goal conflicts with the new goal.

If a task asks for delivery, publication, external updates, repository changes, issue/PR creation, email, docs, or any other state-changing outcome, inspect available tools and authenticated connectors before setting the scope. Prefer an end-to-end goal that completes the delivery when tools are available. Do not stop at a plan or handoff merely because the task crosses a service boundary.

If a non-critical detail is missing, choose a conservative default and state it in the goal. If a safety, permission, account-selection, or scope decision is missing, ask before starting.

## Starting the Goal

When the gate passes, first show a concise Goal Preview in chat. Do this before calling `create_goal` so the user can see the target without opening the goal UI.

Use this preview shape:

```text
Goal Preview:
- Objective: ...
- Done: ...
- Scope: ...
- Capabilities: ...
- Verification: ...
- Loop: ...
- Delegation: ...
- Stop: ...

Starting goal now.
```

Keep the preview short. It should expose the execution target and boundaries, not repeat the full goal spec.

Then call `create_goal` with a single objective paragraph or compact list. Include:

```text
Objective:
Done Signals:
Scope:
Capability Inventory:
Verification:
Loop Control:
Delegation Policy:
Stop Rule:
Capture Policy:
```

Keep the objective specific enough that future turns can decide whether to mark the goal complete. Do not use the goal as a broad backlog or open-ended research topic.

## Delegation Policy

Delegation is allowed only when it improves throughput, coverage, or independent review quality. The main agent owns the final judgment, integration, and user-facing answer.

Default policy:

- Internal explorer: allowed for bounded read-only repo questions that can run in parallel.
- Internal worker: allowed only for mechanical or scoped patches with disjoint file ownership.
- external_agent analysis: allowed for read-only second opinion, broad search, risk review, or log/trace summary.
- external_agent sandbox_patch: allowed only for isolated patch experiments with deterministic verification.
- Max concurrency: default 1-2 delegated agents unless the goal has naturally independent slices.
- Evidence: every delegated result must return findings, file paths or artifacts, uncertainty, and recommended next step.

Before delegating, state the main path that remains local and the side task being delegated. Do not delegate the immediate blocker if the main agent can resolve it faster locally.

See `references/delegation-policy.md` for prompt templates and model/tool selection.

## Not-Ready Response

If the request fails the gate, respond in this shape:

```text
Not ready to start.
Missing:
- ...

Assumption that would make it ready:
- ...

Question:
...
```

Ask at most two questions. If defaults are safe, use them and start.

## Capture Policy

At the end of a goal, propose durable capture only when the result changes a future default behavior:

- Use memory for reusable repo-specific or workflow-specific rules.
- Use AGENTS.md for short routing or control-surface guidance.
- Use a skill when the workflow is repeated, procedural, and benefits from triggerable instructions.
- Use a knowledge vault when the content needs human-readable context, links, or longer-form notes.

Do not capture one-off historical detail unless the user asks.
