→ S-137106

# Story LIMBO: an unproved Story with no live Task is reported by a flow rule
parent: F-0346

type: story
parent: F-0346

STORY LIMBO as a flow-conservation rule: an unproved Story under a ranked, unparked Feature with no Task in a live state (Ready/Building/Review/Landing) is reported per tick, counted in the LIMBO line, and listed with its rule name (a `kernel.flow` rule; replaces the proposed knob kernel.limbo.stories). Generic: no product named in code.

## Acceptance
- A Story with no live Task under a ranked, unparked Feature is reported as LIMBO by the rule (test).
- A Story with one live Task is not (test).
- A Story under a parked Feature is not (test).
