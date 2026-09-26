# NEXT drops S1/S2 bugs silently (busy, blocked, Active, over limit) — show a WAITS row with the reason

asf/feeder/rows.py bug_rows() drops a decided S1/S2 Bug from NEXT with no row and no reason in four cases: a live session (busy, rows.py:358), blocked (360), Active with no branch row (363), and over the attempt limit (367). Operators and peer sessions then read the missing row as the Bug being lost. That happened on 2026-09-26 with B-1378, whose fix session was in fact running.

Expected: each case gives a WAITS row with its reason, for example "B-1378 — WAITS ON session fix-bug-b-1378 running" or "held: 4 attempts, over the limit". At minimum do this for S1/S2.

Test: an S1 Bug in each of the four cases → a row with its reason.

## Question
Which Epic is this under? No open Epic shares a title word with it.
