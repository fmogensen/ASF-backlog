→ F-0314

# Open PRs for items already Closed on the trunk are never closed or reaped
parent: E-0001

Bug. Open PRs whose item is already Closed (landed on main through another PR) are never reaped: #766 (T-0266, Closed via a102bc8 / PR #487, PR open since 10-05, log: "stray worker/T-0266 1 patch(es) not on origin/main, never landed - no worktree" every tick) and #1082 (T-0833, Closed, rule reconciled). The PR is green and mergeable but the item card is Closed, so the lane never lands it and nothing closes it; the stray line repeats each tick. This is a leftover on the factory floor, not a pending landing.

## Acceptance
- [ ] An open factory PR whose item card is Closed and whose item id is named by a commit on the trunk is closed by the PR-hygiene pass with a comment naming the trunk commit
- [ ] Its branch is archived or deleted per branch retention, and the `stray` line stops appearing for it
- [ ] A PR whose item is Closed but whose diff is NOT carried by the trunk is not closed; the pass reports it once as needing a decision
- [ ] A test covers both cases: Closed item with a trunk commit naming it (PR closed) and Closed item with no such commit (PR left, one report)
