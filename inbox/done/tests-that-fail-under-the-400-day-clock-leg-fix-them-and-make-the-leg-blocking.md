→ B-225513

# Tests that fail under the +400 day clock leg: fix them and make the leg blocking
parent: F-0357

Type: Bug

Under the CI clock leg shifted +400 days (added by T-210358, report-only for now), these modules fail and pass unshifted, because they read the real clock or pin real dates: tests/test_quota_headroom.py (several status and session-limit cases), tests/test_quota_stale.py, tests/kernel/test_gh_budget.py (plan reuse under one tick old), tests/test_groom_publish.py (commit date and push), the groom evidence test that every undecided card has a decision line, and the session-limit stop that never reads quota.

## Acceptance
- [ ] Each listed test injects now through asf.clock or uses dates relative to it; none reads the real clock.
- [ ] The +400d clock leg passes the whole suite and is made blocking again in the workflow.
- [ ] tools/clock-baseline.txt is lowered for every file fixed.
