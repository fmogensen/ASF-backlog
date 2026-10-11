# Bug: the floor reaper leaves merged branches, worktrees, venvs, caches and orphan beat loops behind
parent: F-0346

type: bug
parent: F-0346

Bug: the kernel's floor reaper leaves most leftovers behind. A console cleanup on 2026-10-11 removed or saved by hand:
- 247 local branches already merged or ancestors of main;
- 72 remote branches of merged or closed PRs;
- 8 registered worktrees (6 agent, 2 /tmp verify), all clean and merged;
- 61 stale pipx venvs (796 MB);
- 5 non-git dirs under state/<p>/worktrees, which worktrees.json lists as "not a git worktree" and keeps forever;
- 24 local-only branches with unpushed work, plus 12 commits found only in trunk-merge checkouts (saved to salvage/*);
- an orphan heartbeat loop (asf-heartbeat.sh for coder-t-0537) still running 5 days after its job ended.
Still left: trunk-merge with 109 cached checkouts (5.2 GB) for finished items, read by the paused 0.1 floor's workers/trunkmerge.py.

## Acceptance
- On each watch pass, the reaper: deletes local and remote branches whose PR merged or closed, or whose head is in main; removes clean merged worktrees no live session holds; removes non-git dirs under worktrees/; and stops heartbeat loops of ended jobs (test per category).
- Before deleting, any branch, worktree or cache holding commits on no remote ref is pushed to `salvage/<name>`; nothing unique is ever deleted (test).
- pipx venvs beyond the live one, its previous and N newest are uninstalled (`kernel.retention.venvs`, default 2) (test).
- Finished items' trunk-merge checkouts are removed once their work is on main (or salvaged) (test).
- `asf kernel status` shows floor debt: counts per category (test).
