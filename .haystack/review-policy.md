# Review Policies

## Sandbox and permission-boundary code requires human review
- **Paths**: `codex-rs/windows-sandbox-rs/**`, `codex-rs/protocol/**`, `codex-rs/cli/**`
- **Severity**: critical
- **Reason**: These paths define filesystem/network sandbox boundaries, ACL/filter setup, and permission profile conversions. Small logic changes can silently weaken isolation or break safety guarantees even when tests pass.

## Exec-server process lifecycle changes require human review
- **Paths**: `codex-rs/exec-server/**`
- **Severity**: high
- **Reason**: Exec-server changes can leak child processes, deadlock shutdown, or leave stale cached clients after transport failure. Automated checks may miss runtime races across tokio/runtime and OS process-group behavior.

## Config layering and override semantics require human review
- **Paths**: `codex-rs/config/**`
- **Severity**: high
- **Reason**: Config precedence, strictness, and trusted-layer behavior can create silent security regressions (ignored keys, unintended defaults, managed policy bypass) that compile and often pass unit tests.

## Instructions
- If a change alters behavior from fail-closed to fail-open (or the reverse) in config validation, sandbox setup, or permission enforcement, require human judgment that the tradeoff is intentional and documented.
- If a PR changes security descriptions or readiness claims, a human must confirm the implementation and tests actually cover the claimed scope (for example direct sockets versus resolver-mediated traffic).
- If a change redefines meaning of optional states or sentinel values (such as None/default/disabled), require human review to confirm downstream providers and callers still preserve intended behavior.
