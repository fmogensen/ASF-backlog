# stories-first gate deadlocks: declared Stories are minted only when a plan lands, but the plan waits on Story cards

Severity: S1

The stories-first gate deadlocks every Feature whose spec is written by a session that can't reach the record, which today means every cloud session. `asf.feeder.rows.has_stories` counts Story cards, but a declared `### S-…:` Story becomes a card only in `asf.record.plan_tasks`, when a **plan** lands. The plan waits on the cards, and the cards wait on the plan. On 2026-10-08 this stalled F-0280, F-0303, F-0306 (its sixth Story), F-0293, F-0295, F-0254 and F-0290. Each one re-fires spec-amend, adjudicate or correct sessions that end "needs input" and spend budget for nothing. The stopgap was to mint the cards on the host by hand.

## Acceptance
- When a spec lands on the trunk, the tick's record step mints its declared `### S-…:` Stories as cards under the Feature, before the feeder reads `has_stories`. The ids are checked against the session's claim, exactly as `plan_tasks` checks them. A test covers it: a landed spec declaring two Stories, then one tick, then two Story cards exist and the Feature draws a plan row, not `NO STORIES → SPEC-AMEND`.
- A spec-amend that adds a Story mints that Story the same way on its landing.
- A declared Story whose id is already a card is not minted twice, and a declared id outside the claim is refused by name.
- The `NO STORIES → SPEC-AMEND` row fires only when the landed spec declares no Story at all.

## Question
Which Epic is this under? No open Epic shares a title word with it.
