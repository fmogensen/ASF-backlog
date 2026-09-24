# A network blip on push scores finished work as 'failed: not pushed' and costs a correction round
parent: E-0002

signature: publish refused: fatal: unable to access 'https://github.com/…': Could not resolve host
parent: E-0002

2026-09-24 03:00: a DNS blip ("Could not resolve host: github.com", resolved within a minute) made `publish plan/F-0101 refused`. The session correct-f-0101 then ended `failed: not pushed: 0 uncommitted file(s), 1 unpushed commit(s)`, and its finished work is scored as a failed session that costs a correction round.

## Acceptance
- [ ] A push or publish that fails with a transient network error (DNS resolution, connection refused or reset, timeout, HTTP 5xx from the remote) is retried with backoff within the same tick, and on later ticks, by the tick itself from the session's worktree. The run is recorded as `ended, publish pending`, not failed, and no correction session is launched.
- [ ] Only a non-transient refusal (auth, a rejected non-fast-forward after rebase, a hook refusal) is a failure.
- [ ] Tests classify each error class.
