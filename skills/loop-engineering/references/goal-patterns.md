# Goal Patterns

Use these as compact starting points. Apply the risk minimums and checker rules from `SKILL.md`; do not copy a lower tier over a known higher-risk trigger.

## Read-Only Analysis

```text
Objective:
Investigate [topic/problem] in [repo/system/source] and produce evidence-backed findings plus next steps.

Done Signals:
- Entry points, call path, data flow, or source boundaries are identified.
- Findings cite concrete files, commands, logs, traces, docs, or external sources.
- Direct causes are separated from symptoms and assumptions.
- Residual uncertainty and the next verification step are stated.

Scope:
Read-only in [paths/sources]. No edits or external mutation.

Risk Tier:
Default `T1`; use `T2` for security-sensitive, cross-system, policy-shaping, or high-consequence conclusions.

Capability Inventory:
Use [local files, shell, web, logs, traces, connectors]. Mark unavailable private sources.

Role Topology:
Main agent orchestrates and authors the synthesis. For `T2`, assign one fresh-context read-only checker to challenge source coverage, unsupported claims, and alternative causes.

Evidence Plan:
Use `E2 structural` source references and `E3 field truth` when live traces or system state are available.

Checker Contract:
For `T2`, review the final report snapshot against source quality, claim support, coverage, and uncertainty. Fail on unsupported consequential claims or missing required source families.

Loop Control:
Checkpoint after each distinct evidence family. Default budget is inspect, synthesize, and verify/review. Terminal states: success, no-op, blocked, stalled, exhausted.

Stop Rule:
Stop if required credentials, production access, or unavailable evidence is necessary for a reliable conclusion.

Capture Policy:
Propose durable capture only for a reusable workflow rule.
```

## Implementation

```text
Objective:
Implement [change] in [repo/artifact] and prove the final snapshot meets the declared Done Signals.

Done Signals:
- Existing patterns and baseline behavior are inspected.
- Changes stay inside the named scope and preserve unrelated work.
- Relevant tests, checks, previews, or field readback pass.
- Required checker verdict applies to the exact final snapshot.
- Changed files, evidence, and residual risk are reported.

Scope:
Write only within [paths/modules]. Exclude [areas].

Risk Tier:
Use `T0` for an exact mechanical edit, `T1` for bounded reversible work, `T2` for cross-module/user-visible/control-plane work, and `T3` for critical domains.

Capability Inventory:
Use local filesystem and test tools. Use repository or external connectors only when authenticated and in scope.

Role Topology:
Main agent is orchestrator and may be maker. `T2/T3` requires a separate read-only checker; `T3` also requires a human gate. Delegate maker work only for disjoint scopes.

Evidence Plan:
Prefer `E1 deterministic` tests and exact diffs; add `E2 structural` checks and `E3 field truth` when relevant.

Checker Contract:
For `T2/T3`, review a content-bound final commit/diff/checksum snapshot against correctness, scope, regressions, safety, and evidence coverage. Fail on unmet Done Signals or blocking findings. Any post-review edit invalidates the verdict.

Loop Control:
Inspect -> make -> evidence -> checker -> one repair/re-check cycle. Stop as stalled after two checkpoints add no new evidence.

Stop Rule:
Stop for destructive operations, new credentials, production changes, unavailable required checker, or unresolved product decisions.

Capture Policy:
Propose capture only for a reusable implementation or validation pattern.
```

## Review Or Risk Scan

```text
Objective:
Review [diff/design/logs] for correctness, regressions, security, and missing validation, then report prioritized findings.

Done Signals:
- Findings are ordered by severity and cite exact evidence.
- False positives and uncertainty are explicit.
- Missing tests and unavailable baselines are listed.
- No target artifact is modified.

Scope:
Read-only over [diff/files/logs].

Risk Tier:
Default `T1`; use `T2` for broad, security-sensitive, release-blocking, or architecture-critical review and `T3` when the report gates a critical irreversible action.

Capability Inventory:
Use repository state, diffs, tests, CI, traces, logs, and exact external readbacks as available.

Role Topology:
The target artifact maker is outside this review loop. Main agent is primary checker and report author. For `T2/T3`, assign a separate fresh-context reviewer to validate the report's evidence, severity, and omissions. `T3` requires a human gate.

Evidence Plan:
Use `E1/E2` for concrete regressions and `E3` for live state. Do not present model judgment as deterministic proof.

Checker Contract:
For `T2/T3`, review the final report snapshot. Fail if a high-severity claim lacks evidence, the baseline is wrong, or required risk areas were not inspected.

Loop Control:
Inspect baseline -> inspect change -> draft findings -> independent report check when required. Terminal states distinguish no findings from blocked evidence.

Stop Rule:
Stop if the compared baseline or required private system cannot be established.

Capture Policy:
Propose a reusable review skill or routing rule only when the pattern will recur.
```

## Learning And Synthesis

```text
Objective:
Research and synthesize [topic] into actionable guidance for [workflow/context].

Done Signals:
- Concept boundaries and current practices are established.
- Guidance maps to the user's concrete workflow.
- Immediate use, optional adoption, and non-goals are separated.
- Current or unstable claims cite sources.

Scope:
Use [web/docs/local notes/repo]. Do not create durable files unless asked.

Risk Tier:
Default `T1`; use `T2` when the synthesis changes policy, agent behavior, security posture, or a costly decision.

Capability Inventory:
Use current primary sources, local evidence, and bounded connectors as needed.

Role Topology:
Main agent orchestrates and authors. For `T2`, use one fresh-context checker for source quality, coverage, and unsupported recommendations.

Evidence Plan:
Use `E2 structural` source mapping and `E3 field truth` for current external facts.

Checker Contract:
For `T2`, fail on material uncited claims, missing opposing evidence, or recommendations that exceed the researched scope.

Loop Control:
Checkpoint per source family, synthesize, then independently review when required.

Stop Rule:
Stop when required sources are paywalled, inaccessible, or private and no honest fallback exists.

Capture Policy:
Recommend a skill only if the result changes future default behavior.
```

## Skill Creation

```text
Objective:
Create or update a concise Codex skill for [workflow], delivered at [path].

Done Signals:
- `SKILL.md` frontmatter and instructions validate.
- References are one level deep and loaded conditionally.
- Trigger description states what the skill does and when to use it.
- Interface metadata matches the final skill.
- Fresh-context forward-testing exercises realistic requests.
- The exact final snapshot passes required checks and review.

Scope:
Create only required runtime files. Keep marketplace documentation outside the installed skill folder.

Risk Tier:
Default `T2` because skills change future agent behavior. Raise to `T3` if the skill controls permissions, credentials, production actions, or destructive operations.

Capability Inventory:
Use the skill validator, local artifact checks, and internal subagents for bounded forward-tests. Use external agents only for a declared diversity need.

Role Topology:
Main agent orchestrates and may be maker. Assign one fresh-context checker that did not edit the skill. Require a human gate for `T3` behavior.

Evidence Plan:
Use `E1` validator/file checks plus `E2` policy checks. Treat forward-test behavior as independent judgment, not deterministic proof.

Checker Contract:
Review the content-bound final skill snapshot for trigger fit, instruction consistency, maker-checker integrity, permission safety, and forward-test evidence. Fail on policy contradictions or leaked expected answers. Recheck after every repair.

Loop Control:
Inspect -> edit -> validate -> forward-test -> independent review -> one repair/recheck cycle.

Stop Rule:
Stop if the install path, required subagent, or permission-sensitive behavior cannot be tested safely.

Capture Policy:
The skill is the durable artifact. Additional memory is user-directed.
```

## External Publication Or Delivery

```text
Objective:
Publish, send, upload, create, or otherwise deliver [artifact/change] to [external destination] and report the resulting visible state.

Done Signals:
- Local source content and privacy boundaries are inspected.
- Destination account, permissions, connectors, and overwrite behavior are known.
- The destination is updated when authorized tools are available.
- `E3` readback confirms the expected resource and content.
- Remaining marketplace, cache, approval, or visibility delays are explicit.

Scope:
Write only to [local paths] and [external destination].

Risk Tier:
Default `T1` for reversible delivery, `T2` for public/user-visible or broad updates, and `T3` for destructive overwrite, sensitive data, financial effect, production deployment, or irreversible publication.

Capability Inventory:
Prefer purpose-built connectors or official APIs. Use CLI fallback only when connector coverage is insufficient.

Role Topology:
Main agent orchestrates and normally performs credential-bound publication. For `T2`, assign a read-only checker for package content, privacy, and destination readback. `T3` requires independent review and human approval before mutation.

Evidence Plan:
Use `E2` package/privacy checks before mutation and `E3` destination readback afterward.

Checker Contract:
Review the pre-publication snapshot and final destination identity. Fail on private data exposure, wrong account/destination, missing files, stale readback, or unauthorized overwrite.

Loop Control:
Checkpoint before mutation, after mutation, and after readback. Re-review if published content changes.

Stop Rule:
Stop for missing credentials, account ambiguity, destructive overwrite, private-data risk, or unavailable readback.

Capture Policy:
Propose a skill or routing update only for a reusable publication rule.
```
