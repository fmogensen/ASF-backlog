→ S-137607

# Intake never starves: an inbox note gets an intake seat within a bound
parent: F-0346

type: story
parent: F-0346

Intake never starves: when every seat is busy, inbox notes that need an intake-decide session wait indefinitely (now visible as `INTAKE WAIT`, still no seat). Intake decisions are short and gate all new work, so they must get a seat within a bound.

## Acceptance
- With all seats busy and a note waiting for intake-decide, the note gets a session within `kernel.flow.intake_wait_s` (seed one tick), via a reserved intake seat or by taking precedence over new build launches (test).
- The reserved seat is not used by build/review sessions while no note waits (test).
- `INTAKE WAIT` rows older than the bound appear as a flow violation (test).
