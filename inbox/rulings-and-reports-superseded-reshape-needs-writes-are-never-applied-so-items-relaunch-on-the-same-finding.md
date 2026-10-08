# rulings and reports (superseded, reshape, needs writes) are never applied, so items relaunch on the same finding

Severity: S2

Reported by botseon on 2026-10-08. Rulings and session reports with a machine-readable outcome are never applied:
- T-0353 was ruled "superseded" three times.
- T-0537 got a groom reshape on 2026-10-07 that was never applied.
- T-0436 reported "needs writes: X" with the same two files in three reports.
- B-1557 hit the same duplicate finding three times.

## Acceptance
- A ruling `superseded_by: <id>` (or "superseded") applies on the next tick: the item is retired with the ruling cited, and no further session launches. A test covers it.
- A report line `needs writes: <paths>` widens the Task's `writes:` (through the widening rule) or refuses by name. It is never left for a relaunch to repeat. A test covers it.
- A groom reshape answer is applied by the next replan. A test covers it.
- A session report repeating an already-recorded finding counts as `same` toward the cap, not as new.

## Question
Which Epic is this under? No open Epic shares a title word with it.
