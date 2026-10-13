# A revert PR that never merges still marks the item reverted: endless empty rebuilds
parent: E-0006

Type: Bug

When the kernel opens a revert PR for a red trunk, it records the revert as done at once (kernel_reverted on the card, note "reverted off main — back to Ready"). If the revert PR never merges, the item is still on the trunk but the kernel keeps relaunching builds for it. Each build has an empty diff, its PR is closed as "changes nothing", and the loop repeats. Observed 2026-10-13 on T-166581 (PR #1678 merged, revert PR #1681 closed unmerged): three rebuild launches in 40 minutes while the same tick logged "SATISFIED T-166581 on main". An operator answer also relaunched it instead of ending it; the console ended the loop with `asf retire --landed 1678`.

Acceptance:
- kernel_reverted is recorded only when the revert PR has merged. A revert PR that is closed unmerged, or still open, leaves the item Done.
- An item whose acceptance is satisfied on the trunk and whose merge is still an ancestor of the trunk is never relaunched.
- A build that ends with an empty diff against the trunk marks the item Done (already on the trunk), not Ready.
- Test: a replay with RevertPR opened then closed unmerged yields no Launch for the item.
