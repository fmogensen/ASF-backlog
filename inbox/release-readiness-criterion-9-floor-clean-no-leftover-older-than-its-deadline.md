# Release readiness criterion 9: floor clean — no leftover older than its deadline

Operator 2026-10-06: "is the factory floor cleanup something we need to have in place as part of public release?" Answer: yes, for what a public user sees or pays for.

Add criterion 9 to `asf release-readiness` (asf/…release readiness module; product yaml `release:` thresholds):
"Floor clean — no leftover older than its deadline", red when any of these exists past its limit:
- an open PR the lane marks STALE (default limit 3 d);
- a CI run whose batch ref is gone or whose PR is closed, still queued/running (limit 30 min);
- a cloud run with no heartbeat for 2 x `cloud.heartbeat_min` (see the cloud-heartbeat item);
- a Task in one stage past 3 x its `stage_limits` value;
- a remote head with no owner past `branch_retention`.
Evidence column lists counts and the oldest of each. Thresholds are `release.floor.*` keys with defaults.

Acceptance: hermetic tests per leftover kind (red past limit, green under it); `asf release-readiness` prints the row; `asf doctor` mirrors it.
Depends on: "Stale means act, not flag" (the actions that make this criterion pass).
