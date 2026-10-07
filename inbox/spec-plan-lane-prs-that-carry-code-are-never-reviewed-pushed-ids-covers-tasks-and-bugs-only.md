# Spec/plan-lane PRs that carry code are never reviewed (pushed_ids covers Tasks and Bugs only)

Parent: E-0001
severity: S2

Spec/plan-lane PRs that carry code are never reviewed. `pushed_ids()` in asf/feeder/rows.py recognises only Task and Bug items. A PR on a Feature-keyed branch (spec/F-xxxx, plan/F-xxxx) whose diff touches code therefore gets "WAITS ON landing" while the lane wants "review round 1", and no review session ever launches. Docs-only spec PRs merge as `review: none`; these don't. Seen 2026-10-07 on ASF: #941 (F-0040), #897 (F-0120), #885 (F-0133) and #322 were green and CLEAN for 1 day or more.

## Acceptance
- A Feature-keyed branch PR whose diff touches non-doc paths gets a PUSHED → REVIEW row that launches a review session. Tested.
- A docs-only spec PR still lands as `review: none`. Tested.
- A review verdict on the head lets the lane merge it. Tested.
- No PR sits in "round N wanted" without a launchable row for more than N minutes: the watchdog breaches and doctor names it. Tested.
