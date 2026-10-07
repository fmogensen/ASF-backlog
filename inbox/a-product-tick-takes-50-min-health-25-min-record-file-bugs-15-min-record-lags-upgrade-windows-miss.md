# A product tick takes ~50 min (health 25 min, record:file-bugs ~15 min): record lags, upgrade windows miss

Parent: E-0001
severity: S2

A botseon tick took 50 min (pid 20126, 08:49–09:39Z on 2026-10-07), so record and ingest run only about every 45-50 min. Landings take that long to close. An upgrade window can't find a gap between ticks, and the 3-minute move bound fails on its first attempt. Step times for that tick:
- health: 1493 s. Its sub-steps print no timings. The log shows "worktrees: reaped 14 — deleting …", 10 stale-archive lines and many INPUT rows.
- record-tail: 923 s. `[record:file-bugs]` alone takes 873–1111 s on each of the last 3 ticks.
- wave: 130 s + 305 s.
- record: 74 s.

## Acceptance
- Every sub-step of `health` and `record-tail` prints `[health:<sub>] <s>s`, like `[record:<sub>]` does, so the slow part is named. Tested.
- `record:file-bugs` stays within 60 s on a record of botseon's size (fixture of about 1,500 items). It does incremental work (only cards or signatures changed since the last pass) with a bounded number of host calls per pass. Tested with a call-count fake.
- Worktree reaping runs in the background, or is capped per tick (configurable, default 3), so health never blocks on deleting large trees. Tested.
- One tick of the default step set finishes within `tick.budget_s` (configurable, default 600). A step over its budget logs a watchdog breach naming the step. Tested.

## Question
This reads as a defect. A Bug carries a signature — add signature: <the failing test or error line>, or paste that line into the body (an `Error:` line or a `file:line › test` line is read as one); or an ## Acceptance list if it is new work.
