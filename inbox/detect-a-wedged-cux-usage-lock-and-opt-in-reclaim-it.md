# Detect a wedged cux usage lock and (opt-in) reclaim it

cux's ~/.cux/.lock wedged twice on 2026-09-27: held 6 h 19 m by a `cux --dangerously-skip-permissions` child of an interactive cux wrapper (01:28–07:50), then 27 m by another (07:5x–08:18). Each time `cux usage refresh` timed out ("acquire lock: timeout"), quota went stale, and launches were throttled; killing the childless holder and moving the lock restored polling at once.

Ask:
1. Detect: the quota adapter reports "lock held by pid N (age, parent)" when refresh times out; `asf status` Quota row and `asf doctor` show "cux lock wedged N min" instead of only "stale since".
2. Reclaim, opt-in (`quota_guards.reclaim_cux_lock: true`, default off): only when the holder is a cux child (not a wrapper, not a claude process), older than 15 min, with no children of its own — SIGTERM it, move the lock aside to .lock.stale-HHMM, refresh once, and log the action. Never touch a wrapper or a live Claude session.
3. Upstream: the real fault is cux holding its global lock across a long-lived child; report to cux so the lock is scoped to the refresh.

## Question
Which Epic is this under? No open Epic shares a title word with it.
