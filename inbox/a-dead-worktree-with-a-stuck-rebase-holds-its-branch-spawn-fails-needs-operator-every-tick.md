# A dead worktree with a stuck rebase holds its branch; spawn fails NEEDS OPERATOR every tick

2026-09-26 18:05 botseon: correct-t-0349 failed to spawn every tick with NEEDS OPERATOR — `git worktree add -B cloud/T-0349 …` → "fatal: 'cloud/T-0349' is already used by worktree at …/worktrees/coder-t-0349". That worktree was detached at a factory WIP commit (f0690aeb2, B-0094) with a rebase-merge in progress whose head-name was refs/heads/cloud/T-0349 — a stuck rebase holds the branch. No live session owned it.

Fix in code: before spawning on a branch, when the holding worktree has no live session: archive any commits not on origin (archive/<branch>-wip-<sha9>, push with --no-verify since it's an archive ref), `git rebase --abort`, remove the worktree, then spawn. Never surface this as NEEDS OPERATOR. Test: a dead worktree with a rebase in progress on the branch is archived+cleared and the spawn succeeds.

Hand-cleared this once (archived f0690aeb2 to origin archive/cloud-T-0349-wip-f0690aeb2, aborted rebase, removed worktree).

## Question
This reads as a defect. A Bug carries a signature — add signature: <the failing test or error line>; or an ## Acceptance list if it is new work.
