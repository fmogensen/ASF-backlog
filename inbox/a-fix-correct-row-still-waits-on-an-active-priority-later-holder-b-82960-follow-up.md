# a FIX → CORRECT row still waits on an Active priority-later holder (B-82960 follow-up)
parent: E-0001

signature: footprint correction waits on Active task of a priority later Feature
parent: E-0001
severity: S1

After B-82960 (#1239, ASF 0.1.265), `asf next --product asf` still shows `FIX → CORRECT | T-81129 | F-0312 | WAITS ON T-0056 (parked: F-0023 later)`. T-81129 has no `after:`: the wait is a `footprint` correction's stored `waits` verdict (asf/feeder/rows.py footprint_row), produced by the widening rule's open_footprints (asf/tick/widen_footprint.py), which still counts every Active or correction-pending Task, parked under a `priority: later` card or not. T-0610 waits on parked T-0058 the same way. Fix: PR #1244 (fix/B-82960-parked-holder-row).

## Acceptance
- The widening rule skips a Task parked under a `priority: later` card, so a FIX → CORRECT row whose only holder is an Active later-priority Task with an intersecting footprint widens and launches. A test covers it: tests.test_widen.RefusalWidenTests.test_a_refused_path_a_parked_later_task_writes_widens_and_corrects.
- A stored `waits` verdict on a parked owner is stale: the row waits on the rule (`WAITS ON widen_footprint`), unless the corrected Task is parked too. A test covers it: tests.test_feeder.AClosedItemHoldsNoWidenedRow.test_an_owner_parked_under_a_later_feature_is_not_waited_on.
