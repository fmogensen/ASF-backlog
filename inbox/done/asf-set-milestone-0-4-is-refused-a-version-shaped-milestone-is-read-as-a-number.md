→ B-202833

# asf set milestone=0.4 is refused: a version-shaped milestone is read as a number
parent: E-0006

Type: Bug

`asf set <card> milestone=0.4` is refused with "milestone=0.4 — not declared in milestones.md (declared: 0.3, 0.4)". The value is parsed as a number and compared with the declared names, which are text. `milestone="0.4"` works. A version-shaped milestone name is the common case.

Acceptance:
- `asf set <card> milestone=0.4` and `asf new ... --set milestone=0.4` accept a declared milestone whose name looks like a number, and store it as text.
- Test: a record declaring `0.4`; both commands succeed unquoted and `asf release-readiness --milestone 0.4` counts the card.
