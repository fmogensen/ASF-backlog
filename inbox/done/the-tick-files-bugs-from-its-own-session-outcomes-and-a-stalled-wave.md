→ F-0103

# The tick files Bugs from its own session outcomes and a stalled wave
parent: E-0002

Self-improvement loop: the failures above (not pushed, empty branch, dead pid, a harvest ledger that never closed and blocked the wave for hours on 2026-09-23) were found by an operator's assistant reading the ledger and tick log by hand. `file-bugs` files Bugs from CI, refusals and rule violations, but not from the factory's own session outcomes or from a stalled wave.

Expected: the tick files or bumps a Bug by signature whenever:
- a session-outcome class exceeds a rate (e.g. `not pushed` over 10% of sessions in 24h);
- the same Task fails the same way twice;
- the wave reports `nothing to launch` for N ticks while New Tasks exist, naming the gate that holds them (e.g. `WAITS ON` a Closed item).

The factory then fixes itself through its own pipeline instead of waiting for someone to look.

## Question
This reads as a defect. A Bug carries a signature — add signature: <the failing test or error line>; or an ## Acceptance list if it is new work.

## Acceptance
- [ ] The tick files or bumps a Bug by signature when a session-outcome class exceeds a rate over 24h (default: `not pushed` > 10%).
- [ ] The same Task failing the same way twice files or bumps a Bug naming the Task and the result.
- [ ] N consecutive ticks with `wave: nothing to launch` while New Tasks exist file a Bug naming the gate that holds them.
- [ ] Tests for each trigger; the thresholds are product config.
