→ B-0380

# Spawn reuses or recreates a leftover worktree with no live session instead of failing every tick
signature: worktree already exists on spawn of a job with no live session
severity: S2

Parent: E-0001

A session spawn fails on every tick with "worktree already exists: <state>/worktrees/<job>" when an earlier spawn attempt left the worktree behind and no process holds it. The job never launches until someone removes the worktree by hand. Seen 2026-10-06 on botseon: correct-t-42278 failed every tick after the 21:05 attempt. Its worktree was clean, matched origin/cloud/T-42278 exactly, and had no live session.

## Acceptance
- When a spawn's target worktree exists and no live session holds it, the spawn handles it in one of three ways:
  - (a) Clean and at the origin branch head: the spawn reuses it.
  - (b) Clean but at a different head: the spawn resets it to the origin head, or removes and recreates it.
  - (c) Dirty: the spawn saves the dirty state to a recovery ref or stash first, then recreates it, and logs the ref.
- A worktree held by a live session (by pid or heartbeat) is never touched. The spawn defers, with that reason in the log.
- No spawn fails twice in a row for "worktree already exists". Tests cover a, b, c and the live-session case.
