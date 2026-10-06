# Spawn reuses or recreates a leftover worktree with no live session instead of failing every tick

Parent: E-0001
severity: S2

A session spawn fails on every tick with "worktree already exists: <state>/worktrees/<job>" when an earlier spawn attempt left the worktree behind and no process holds it. The job never launches until someone removes the worktree by hand. Seen 2026-10-06 on botseon: correct-t-42278 failed every tick after the 21:05 attempt. Its worktree was clean, matched origin/cloud/T-42278 exactly, and had no live session.

## Acceptance
- When a spawn's target worktree exists and no live session holds it, the spawn handles it in one of three ways:
  - (a) Clean and at the origin branch head: the spawn reuses it.
  - (b) Clean but at a different head: the spawn resets it to the origin head, or removes and recreates it.
  - (c) Dirty: the spawn saves the dirty state to a recovery ref or stash first, then recreates it, and logs the ref.
- A worktree held by a live session (by pid or heartbeat) is never touched. The spawn defers, with that reason in the log.
- No spawn fails twice in a row for "worktree already exists". Tests cover a, b, c and the live-session case.

## Question
This reads as a defect. A Bug carries a signature — add signature: <the failing test or error line>, or paste that line into the body (an `Error:` line or a `file:line › test` line is read as one); or an ## Acceptance list if it is new work.
