# Goal Patterns

Use these patterns as starting points. Keep the final `create_goal` objective compact.

## Read-Only Analysis

```text
Objective:
Investigate [topic/problem] in [repo/system/source] and produce evidence-backed findings plus next steps.

Done Signals:
- Entry points, call path, data flow, or source boundaries are identified.
- Findings cite concrete files, commands, logs, traces, docs, or external sources.
- Direct causes are separated from symptoms, cleanup noise, and assumptions.
- Residual uncertainty and next verification step are stated.

Scope:
Read-only in [paths/sources]. No code edits unless the user later asks.

Verification:
Use source references, local command output, trace/log evidence, and citations as applicable.

Loop Control:
Verification level is usually `L2 structural` or `L3 field truth`, depending on the evidence source. Use checkpoints after each distinct source family is inspected. Terminal states: success with findings, no-op with evidence, blocked on missing source, stalled after repeated searches add no new evidence, exhausted when the agreed search budget is consumed.

Delegation Policy:
Allow internal explorer for independent code search. Allow external_agent analysis for second-opinion review only when scope and tools are bounded.

Stop Rule:
Stop if required credentials, production access, or unavailable traces are necessary.

Capture Policy:
Propose memory or AGENTS.md update only if this reveals a reusable workflow rule.
```

## Implementation

```text
Objective:
Implement [change] in [repo/artifact] and verify it with the smallest reliable checks.

Done Signals:
- Existing patterns are inspected before edits.
- Changes are limited to the requested behavior and named scope.
- Relevant tests, commands, previews, or artifact checks pass, or failures are reported.
- Changed files, verification, and residual risk are summarized.

Scope:
Write only within [paths/modules]. Preserve unrelated user changes.

Capability Inventory:
Use local filesystem tools for edits, repository tools for branch/commit/PR operations when available, and external service connectors only when authenticated and relevant.

Verification:
Run [specific tests/commands] when available. If not available, explain the closest check.

Loop Control:
Prefer `L1 deterministic` tests, exit codes, or artifact checks; fall back to `L2 structural` checks only when deterministic execution is unavailable. Default budget is inspect, edit, verify/repair. Stop as stalled if two verification attempts fail without new evidence.

Delegation Policy:
Allow internal worker only for disjoint file ownership. Allow explorer for pattern lookup. Use external sandbox_patch only for isolated experiments.

Stop Rule:
Stop if destructive operations, new credentials, production changes, or unclear product decisions are required.

Capture Policy:
Propose capture only if the implementation creates a reusable coding or validation pattern.
```

## Review Or Risk Scan

```text
Objective:
Review [diff/design/logs] for correctness, regressions, security, and missing validation, then report prioritized findings.

Done Signals:
- Findings are ordered by severity and cite exact evidence.
- False positives and uncertainty are called out.
- Missing tests or validation gaps are listed.
- No unrelated rewrites or implementation changes are made unless requested.

Scope:
Read-only over [diff/files/logs].

Verification:
Use source references, command output, CI status, tests, or trace/log snippets.

Loop Control:
Prefer objective evidence (`L1`/`L2`) for concrete regressions and use independent judgment (`L4`) only for risk prioritization or design critique. Terminal states must distinguish no findings from blocked baseline or unavailable evidence.

Delegation Policy:
Prefer one external_agent analysis job for independent second opinion when risk is high. Main agent verifies important claims.

Stop Rule:
Stop if the review needs unavailable private systems or cannot establish the compared baseline.

Capture Policy:
Propose a reusable review skill or AGENTS.md route only if this review pattern will recur.
```

## Learning And Synthesis

```text
Objective:
Research and synthesize [topic] into actionable guidance for [user workflow/context].

Done Signals:
- Concept boundaries and current best practices are established.
- Guidance maps to the user's concrete daily work.
- Recommendations distinguish immediate use, optional adoption, and non-goals.
- Sources or evidence are cited when external/current facts matter.

Scope:
Use [web/docs/local notes/repo] as needed. Do not create durable files unless asked.

Verification:
Use current sources for unstable facts and local memory/repo evidence for user-specific mapping.

Loop Control:
Use checkpoints per source group or concept boundary. Treat synthesis as assisted if it depends on model judgment (`L4`); do not present it as deterministic. Stop when recommendations map to the target workflow or when source access is blocked.

Delegation Policy:
Allow external_agent analysis only for independent literature/source scan when coverage matters.

Stop Rule:
Stop if the topic requires paywalled, inaccessible, or user-private sources.

Capture Policy:
Recommend a skill only if it changes future default behavior.
```

## Skill Creation

```text
Objective:
Create and validate a concise Codex skill for [workflow], installed or delivered at [path].

Done Signals:
- Skill has valid `SKILL.md` frontmatter and concise body.
- References are one level deep and loaded only when useful.
- Trigger description states when to use the skill.
- Validation script passes.
- Installation path or deliverable limitation is reported.

Scope:
Create only required skill files. Avoid extra README, changelog, or broad documentation.

Verification:
Run the skill validator. Forward-test with a subagent only when useful and bounded.

Loop Control:
Use `L1 deterministic` validation for frontmatter and file shape, then optional `L4 independent judgment` forward-testing for behavioral fit. Terminal states include success, blocked on install permission, stalled after repeated validator failures without a new fix, and exhausted when the agreed validation budget is spent.

Delegation Policy:
Allow one internal subagent for realistic forward-testing after local validation.

Stop Rule:
Stop if the target install path requires approval that is denied.

Capture Policy:
The skill itself is the durable artifact. Additional memory is optional and user-directed.
```

## External Publication Or Delivery

```text
Objective:
Publish, send, upload, create, or otherwise deliver [artifact/change] to [external destination] and report the resulting URL, identifier, or visible state.

Done Signals:
- Local artifact or source content is inspected and ready.
- Available connectors, CLIs, credentials, accounts, remotes, and destination permissions are inventoried before deciding the plan.
- The external destination is updated when authenticated tools are available.
- The result is verified by reading the published resource, repository metadata, URL, issue, PR, email draft, or destination state.
- Any remaining manual marketplace, cache, approval, or visibility step is explicit.

Scope:
Write only to [local paths] and [external destination]. Do not change unrelated local files or remote resources.

Capability Inventory:
Prefer purpose-built MCP connectors or official APIs. Use CLI fallback only when connector coverage is insufficient. If account selection, credentials, or publication permissions are missing, stop and ask rather than simulating completion.

Verification:
Confirm the external resource exists and contains the expected files or content. For GitHub publication, verify repository metadata and key files after push or contents API writes.

Loop Control:
Use `L3 field truth` by reading back the external destination after mutation. Checkpoints should happen before mutation, after mutation, and after readback. Stop as blocked rather than simulating success when credentials, account choice, approval, or service-side visibility is missing.

Delegation Policy:
Do not delegate by default. External publication is usually a main-agent responsibility because it combines local artifact ownership, credentials, and final user-facing state.

Stop Rule:
Stop if credentials, account selection, destructive overwrite, private data exposure, or service-side approval is required and not already authorized.

Capture Policy:
Propose skill or AGENTS.md updates only if this reveals a reusable publication or connector-selection rule.
```
