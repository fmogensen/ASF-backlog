→ B-200581

# Trunk-wide red is blamed on the newest merge: wrong revert, fix rounds and an archive; plus three more clock reads
parent: F-0346

Type: Bug

On 2026-10-13 main went red because asf/ci_census.py read its 14-day window from the wall clock instead of the `now` it was given, so two tests with fixtures dated 2026-09-29 began failing when the date rolled over (fixed in PR #1682). The kernel then blamed the newest merge: it opened a revert of an innocent PR (T-166581, PR #1678), although the same tests failed on the revert PR itself and on unrelated PRs. Earlier it archived T-160210 for "red tests after 3 fix rounds" on the same failure.

Acceptance:
- Before reverting a merge for a red trunk, the kernel checks that the failing test passes on the parent commit (or on the revert PR). A failure that is also red without the merge is classed as trunk-wide or environmental: no revert, no fix rounds charged to open PRs, launches held, one Bug filed naming the test.
- A PR whose only red is a test that is also red on the trunk is not charged a fix round and is not archived for it.
- The remaining wall-clock reads get an injected now: asf/ci_stall.py:206, asf/ci_cancels.py:354, asf/doctor.py:2268; tests/test_tick_steps.py is checked for date sensitivity. A lint refuses new direct metrics.today() reads outside one clock helper.
- Tests: a replay with a trunk-wide red yields no RevertPR and no fix round; each of the three modules has a test that passes with the clock moved 30 days.
