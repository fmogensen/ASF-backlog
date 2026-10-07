→ F-0284

# A CONFLICTING PR gets no CI run; its priority land request shows 'not started' forever instead of triggering a rebase

Parent: E-0001
severity: S2

A PR that GitHub marks CONFLICTING gets no `pull_request` CI run on new pushes. The merge queue still shows its land request as "pending — gate (not started) …" indefinitely, and the CI queue never lists it, so a priority land request waits on a run that will never start. Seen 2026-10-07 14:2x on botseon: PR #1143 (a goal Feature, `land --priority`) at head 0c14763cc had mergeable=CONFLICTING and 0 runs, and the last run was on an older head.

## Acceptance
- The merge queue and the lane read `mergeable` for each land-requested or PR_OPEN item. CONFLICTING turns the lane to a conflict correction: the lane's own rebase onto the trunk first (mechanical), and a session only if the rebase conflicts. It is never "pending (not started)". Tested.
- `asf status` Land requests shows "conflicting with <trunk> — rebase queued" for such a PR, and `asf ci queue` names it as "no run possible: conflicting". Tested.
- A land request whose head has no run and no queue entry for more than N minutes (configurable) raises one watchdog breach naming the reason (conflicting, draft, workflow filter). Tested.
