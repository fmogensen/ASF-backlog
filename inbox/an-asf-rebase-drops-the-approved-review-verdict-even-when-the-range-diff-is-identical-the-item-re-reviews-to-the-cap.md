# an ASF rebase drops the approved review verdict even when the range-diff is identical; the item re-reviews to the cap

Severity: S2

Reported by botseon on 2026-10-08. After ASF rebases a reviewed branch, the approved verdict is lost even when `git range-diff` shows every commit identical (`=`). B-1496 was approved at e658ac46, ASF rebased it to e86a06c4a (range-diff `=`), and it was re-reviewed up to the daily cap. T-0422 was approved at ea51f5fb7, rebased to 011641ce9, and went back to review round 1.

## Acceptance
- A rebase whose range-diff against the approved head is all `=` carries the approval to the new head (recorded with both shas). No review row is drawn. A test covers it.
- Any `!` or added or removed commit in the range-diff still requires a new review (the existing behaviour stays green).
