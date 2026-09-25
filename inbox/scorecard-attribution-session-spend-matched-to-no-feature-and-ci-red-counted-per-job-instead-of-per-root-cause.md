# Scorecard attribution: session spend matched to no Feature, and CI red counted per job instead of per root cause
parent: E-0001

Bug-shaped Feature: the scorecard's attribution gaps, measured on its first run (2026-09-25).

- Session spend matched to no Feature: the largest single row of "where the money went" on the factory product ($170 of $807 over 14 days) and $81 on the other product. Sessions whose `item` is a Bug under an Epic, or empty, reach no Feature. Attribute by the job's branch and by the Bug's own text naming a Feature (the scorecard already does the latter for bug counts).
- CI red is counted per job: one broken shared step makes a dozen `ci-job:*` causes (13 over threshold on one product). Group red jobs by their failed step / first failing line, so one root cause is one card.
- A migrated card's "created" is the migration day, so its lead time is measured from then.

## Acceptance
- [ ] unattributed session spend falls below 10 % of the window on both products
- [ ] a fixture with five jobs failing on one step yields one cause
