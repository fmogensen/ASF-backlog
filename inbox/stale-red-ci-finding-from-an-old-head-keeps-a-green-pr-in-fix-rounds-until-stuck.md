# Stale red-CI finding from an old head keeps a green PR in fix rounds until Stuck
parent: F-0346

Type: Bug

A red-CI finding from an older head counts as a fix round, and still counts after the PR goes green on its current head. On 2026-10-12, T-159852 went Stuck ("changes requested after 5 fix rounds"). Its latest finding was a red "tests (3.12)" part 2, yet every check on PR #1645 head fdaade599 was green and the PR was CLEAN. T-154345 showed the same pattern earlier in the day.

Acceptance:
- A CI finding carries the head sha it was measured on. When the current head's required checks are green, the kernel drops that finding and re-judges the item (review or land), rather than spending or counting a fix round on it.
- Test: a replay with a red finding on head A, then green checks on head B, re-judges to review or land, not Stuck.
