# Delegation Policy

Use delegation to improve throughput, coverage, or independent review quality. Do not delegate by default.

## Decision Gate

Delegate only when at least one condition is true:

- The side task can run safely in parallel while the main agent continues non-overlapping work.
- Broad read-only exploration benefits from a second search path.
- Independent review, risk scan, or log summary can return concrete evidence.
- A mechanical patch has a disjoint write set and deterministic verification.

Do not delegate when:

- The next critical-path decision is blocked on the result and the main agent can do it locally.
- The task requires final architecture judgment or user-facing conclusion.
- The implementation is tightly coupled across files that must be reasoned about together.
- A deterministic local command is faster and more reliable.
- Delegation would mostly shift work instead of reducing critical-path cost.

## Internal Sub-Agents

Use internal sub-agents when the task should stay inside the Codex runtime and workspace.

Explorer prompt:

```text
Read-only task. Answer this specific question: [question].
Scope: [files/directories/systems].
Excluded: [areas].
Return: findings, file paths or line references, evidence, uncertainty, and recommended next step.
Do not modify files.
```

Worker prompt:

```text
Implement only this bounded change: [change].
Owned write scope: [files/modules].
You are not alone in the codebase; do not revert unrelated changes and adapt to existing edits.
Return: changed files, tests or checks run, behavior changed, residual risk.
```

Prefer explorers for independent code questions and workers for disjoint implementation slices.

## external_agent MCP

Use external agents for a different model family, independent second opinion, broad read-only review, or isolated sandbox patch.

Analysis prompt:

```text
Mode: analysis.
Provider/model: [cursor simple bounded task | claude complex/risk-sensitive task].
Repo/path: [absolute path].
Scope: [files/directories/logs/time window].
Allowed MCP tools: [exact allowlist if any].
Task: [specific read-only question].
Return: prioritized findings, evidence, uncertainty, and recommended next step.
Do not modify files.
```

Sandbox patch prompt:

```text
Mode: sandbox_patch.
Provider/model: [provider/model].
Repo/path: [absolute path].
Write scope: [files/modules].
Task: [isolated patch].
Verification: [deterministic checks].
Return: changed files, verification output, integration notes, and residual risk.
```

Model selection:

- Cursor Composer tier: simple code search, config inventory, log summary, bounded call-path tracing.
- Claude strongest available tier: complex architecture analysis, subtle bug diagnosis, security-sensitive review, or conflicting evidence synthesis.
- Omit external delegation when the main Codex context or direct integration matters more than independent perspective.

MCP safety:

- Pass exact `allowed_mcp_tools`; do not rely on prompt text alone.
- Use `cursor_force_approve_mcp_tools=true` only for trusted read-only diagnostics with explicit environment, service, time window, query, redaction, and row limits.
- Treat external output as evidence, not authority.

## Wait And Merge

After delegating:

- Continue local non-overlapping work.
- Wait only when the next main step requires the result.
- Prefer one patient wait over repeated short waits.
- Verify important claims before acting on them.
- Close completed agents when no longer needed.

Merge result format:

```text
Delegated Result:
- Agent:
- Scope:
- Findings:
- Evidence:
- Uncertainty:
- Recommended next step:
- Main-agent verification:
```
