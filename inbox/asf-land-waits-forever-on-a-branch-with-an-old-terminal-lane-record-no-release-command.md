# asf land waits forever on a branch with an old terminal lane record; no release command
parent: F-0346

Type: Bug

A landing request (`asf land <pr>`) on a branch that ASF did not build waits forever when an older lane record for that branch is in a state that never yields a verdict: MERGED, STALE, BACK or REVIEW. No command releases the record. Reported by the botseon console on 2026-10-12 against the 0.1 lane (four parity PRs blocked). Check whether the 0.2 kernel landing path has the same hole before building.

Acceptance:
- A landing request on an open, green PR is never blocked by a stale or terminal record of an earlier run on the same branch name; the request states why it waits and what releases it.
- A CLI release exists for the case (for example `asf land <pr> --adopt`) and is recorded.
- Test: a branch with a terminal record plus a new open green PR and a land request is batched.
