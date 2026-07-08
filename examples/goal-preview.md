# Goal Preview Example

User request:

```text
Use $loop-engineering to turn this into a goal and start when ready:
investigate why the API smoke test fails and make the smallest safe fix.
```

Expected preview:

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

Expected created goal body:

```text
Objective:
Find the API smoke-test failure and make the smallest safe fix that can be verified locally.

Done Signals:
- The failing test command is run or the reason it cannot run is documented.
- The likely root cause is tied to specific files, configuration, logs, or test output.
- A minimal fix is applied if the issue is local and safe to change.
- The relevant smoke test or closest available verification is rerun.
- Remaining blockers are explicit if verification cannot be completed.

Scope:
The current repository's API smoke test path, directly related implementation files, and test configuration. Exclude unrelated refactors and production-only changes.

Capability Inventory:
Use local shell commands, repository files, and the available test runner. Use a bounded read-only subagent only if the failure spans multiple independent modules. Do not require external services unless the existing test already does.

Verification:
Report the failing command, the passing command or closest available evidence, and changed file references.

Loop Control:
Use `L1 deterministic` verification when the smoke test can run. Checkpoint after initial reproduction, after the patch, and after verification. Default budget is one inspection pass, one patch pass, and one repair pass. Terminal states: success, no-op, blocked, stalled, or exhausted.

Delegation Policy:
Main agent owns diagnosis, patch, and final answer. Allow one internal explorer for read-only call-path or config search if it can run in parallel and return concrete file evidence.

Stop Rule:
Stop if required secrets, remote services, or production access are needed, or if the failure cannot be reproduced and no local evidence supports a patch.

Capture Policy:
Propose durable capture only if the failure reveals a reusable test-running, environment, or delegation rule.
```
