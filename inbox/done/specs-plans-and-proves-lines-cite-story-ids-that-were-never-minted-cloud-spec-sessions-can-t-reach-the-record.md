→ F-0285

# Specs/plans and Proves lines cite Story ids that were never minted (cloud spec sessions can't reach the record)

Parent: E-0001
severity: S2

A spec or plan can claim Story ids that were never minted, and later `Proves:` lines cite them as if they prove something. On botseon, F-1144's plan (docs/superpowers/plans/f-1144.md:32-37) says Stories S-29500..S-29505 "were minted from the spec session's own range and landed with it", but none exist in the record. A cloud spec session can't reach the record, so it wrote ids from its range without minting cards. T-51408's PR #1237 then carried "Proves: S-29504 line 1-9 / S-29505 line 1", which prove nothing. ASF's own spec-amend sessions reported the same limit ("Story cards cannot be minted from any cloud-container session").

## Acceptance
- A spec or plan PR (and the landing gate for any PR) is refused, or turned to a correction, when its text or `Proves:` lines cite a Story or Task id that doesn't exist in the record. The refusal lists the missing ids. Tested.
- A cloud spec or plan session that needs new Stories requests them through the factory: a report section listing the Stories to mint, which the lane mints on the host when it publishes the branch. It never invents ids. Tested end to end with a fake cloud report.
- `asf check` (or doctor) lists ids cited in plans, specs or Proves lines that are missing from the record, per Feature. Tested.
