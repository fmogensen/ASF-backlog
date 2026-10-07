→ F-0287

# A PR is cut into a batch before its review verdict; a later 'changes' verdict launches a correction that pushes onto the batched PR

Parent: E-0001
severity: S2

The merge queue cut a PR into a batch before its ASF review verdict was in. The review then asked for changes, the lane went BACK, a correction session pushed onto the batched PR, and the head moved under the live batch. Seen 2026-10-07 on botseon: #1238 (T-51405, goal F-1144) was cut into batch 43375e4 at 19:51Z at head 83f2e4397. Review round 1 then read "changes" (lane PR_OPEN → BACK), correct-t-51405 launched and pushed a report-only commit 222416324 at about 21:57, and the batch is now at risk of a "head moved" drop.

## Acceptance
- A PR is cut into a batch only when it has an approve verdict for its current head (or `review: none`). The cut is refused with a reason otherwise. Tested.
- A review verdict of changes for a head that is already in a live batch takes the PR out of the batch (withdraw, then re-cut the rest) before any correction launches. No session ever pushes onto a batched PR. Tested.
- A report-only head move (empty tree diff) on a batched PR doesn't drop the batch: the batch keeps the batched sha (the F-0278 deferred line). Tested.
