→ B-145246

# Bug: an external worktree on an item-named branch blocks the item's build
parent: F-0346

type: bug
parent: F-0346

Bug: an external worktree holding a branch named after an item blocks that item's own build. Kernel launches fail with "branch checked out in an external worktree …" and the item goes Stuck(loop), e.g. T-81130, T-129212 and B-138544 on 2026-10-10. The holders were console fix agents on branches like fix/T-129212-<slug>, which merge.factory_only requires to carry an item id, or on the item's own branch.

## Acceptance
- An item whose launch branch is held by an external worktree is shown as a `wait` with the holder's path and branch, never as Stuck after its rounds are spent (test).
- A held branch that isn't the item's launch branch (e.g. fix/<id>-<slug> while the item builds on worker/<id>) never blocks the item's launch (test).
- The wait ends by itself when the worktree is removed (test).
