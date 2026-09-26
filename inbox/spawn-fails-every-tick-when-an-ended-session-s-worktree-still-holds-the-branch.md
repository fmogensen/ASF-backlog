# Spawn fails every tick when an ended session's worktree still holds the branch

Proven 2026-09-26: botseon adjudicate-b-1377 failed to launch on every wave from 11:42 to 12:02 with "spawn failed: git worktree add -B cloud/fix-B-1377 … is already used by worktree fix-bug-b-1377". An ended session's worktree still had the branch checked out. The reaper could not remove it ("contains modified or untracked files"), so the row sat first in Ready to launch while seats were free.

Expected: when spawn needs a branch that an ended session's worktree holds, it hands that worktree to the new job: reuse it in place, or detach its HEAD after proving no work is lost (all commits on origin; dirty files archived to a ref or stash-commit pushed). Never fail every tick. If the worktree holds unpushed work, push it first (the branch is the same item's). The status or next row should show "waits: branch held by worktree X (reason)" instead of "would launch".

Test: an ended session worktree holding the branch plus a new job on the same branch → launches (reuses or detaches); dirty case → the work is preserved and the launch proceeds.

## Question
This reads as a defect. A Bug carries a signature — add signature: <the failing test or error line>; or an ## Acceptance list if it is new work.
