→ closed (groom 2026-09-24, adjudicator, groom-2026-09-24)

# status: a session that exited after a done report shows as dead

The status Agents row and the IN FLIGHT table call a session "dead" (dead pid) when its process has
exited after a normal `REPORT status: done, pushed: yes <sha>`, until the next harvest records it.
An operator reading "1 working, 4 dead" reads it as four crashes.

Expected: a session whose job log ends with a done REPORT is shown as finished / awaiting harvest;
"dead" is kept for a pid gone with no report.
Acceptance: a status test with an exited pid plus a done report shows it as finished, not dead.

## Question
Which Epic is this under? No open Epic shares a title word with it.
