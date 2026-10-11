→ S-157258

# Intake and groom-fill decide by code where the record already has the answer
parent: F-0346

type: story
parent: F-0346

Intake and groom-fill decide by code wherever the record already has the answer. Measured 2026-10-11: intake-decide (151) and groom-fill (110) were 261 of 1471 launches (18%), often re-deriving what a card's own type and parent lines, or its plan Task's Acceptance and writes, already state.

## Acceptance
- A note or card that declares `type:` and a valid `parent:` is minted by code with its parent's priority, and no intake-decide session is launched (test).
- A Task minted from a plan whose Acceptance, writes and after are present passes DoR by code, and no groom-fill session is launched (test).
- `asf kernel waits` or the status view reports the share of intake and groom LLM sessions per day, so the effect is measured.
