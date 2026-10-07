→ F-0275

# A dead review or a lane naming/copies hold relaunches a correct session on an unchanged branch; add asf stop

Parent: E-0001
severity: S2

A correction caused by a dead review, or by a lane naming/copies hold, launches a new correct session on an unchanged branch. When a review session dies without a report (`kind=died`), or the lane's reword bails on trunk-copy commits (`kind=naming`), the lane goes BACK (asf/harvest/lane.py:1451-1456, `correction_turns_back` at :222-239), and the feeder launches `correct-<item>` on the same head. A priority-requested PR is held out of its batch for one to two more CI cycles. The cause is the reviewer's death or lane mechanics, not a defect in the branch. Seen 2026-10-07 08:02-08:07Z on botseon: T-0091 (#1070) was BACK after review-t-0091 died at head 845549bf8; T-0659 (#1122) had a naming/copies hold after the lane's reword bailed. Both relaunched correct sessions. There is also no supported command to end a live session: no `asf stop`, `asf correct --drop` exits 1 once the session is live, and `asf park` kills nothing.

## Acceptance
- A `kind=died` correction on a review run whose branch head has not moved never launches a correct session. The lane re-runs the review once, or re-queues.
- A `kind=naming` hold whose only cause is trunk copies is cleared by the lane's own rebuild first. A session launches only if the rebuild conflicts.
- A lane record whose item has a standing `asf land` request, and whose required checks are success on the head, is not turned BACK by a `died` or `naming` correction.
- `asf stop <job> [--why]` ends a live session (local or cloud) cleanly: the run is ended with reason "stopped by operator", the item is not parked, and the branch is untouched. Once the stop is recorded, `asf correct --drop` on an item with a live correct session names that session and offers the stop.
- Tests:
  - a dead review at head H with a priority request and H green yields QUEUED and no correct launch;
  - a copies-only naming hold is rebuilt with no session;
  - `asf stop` ends the run without parking the item.
