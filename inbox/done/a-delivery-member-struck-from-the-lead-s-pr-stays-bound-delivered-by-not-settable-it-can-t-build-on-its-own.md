→ F-0288

# A delivery member struck from the lead's PR stays bound (delivered_by not settable); it can't build on its own

Parent: E-0001
severity: S2

A delivery member struck from its lead's PR stays bound to the lead. When an adjudication or review finds that a delivery lead's PR doesn't build some members, those members keep `delivered_by: <lead>` and read "WAITS ON delivery <lead>" until the lead lands and another delivery round runs. Neither `delivered_by` nor `delivers` is settable (asf/record/new.py SETTABLE), so the only escape is retiring the members and minting copies. Seen 2026-10-07 22:16 on botseon: adjudicate-t-51405 struck T-51406 and T-51407 from PR #1238 (goal F-1144). Their writes don't overlap #1238, but they couldn't build in parallel.

## Acceptance
- An adjudication or review ruling that a member isn't built by the lead's PR clears the member's `delivered_by` and removes it from the lead's `delivers:` in the same pass, with a History line on both cards. The member then launches as its own Task, subject to its other `after:`. Tested.
- `asf set <task> delivered_by=` (clear), or `asf undeliver <task> --why`, does the same by hand, keeping `delivers:` and `delivered_by:` consistent on both cards. Tested.
