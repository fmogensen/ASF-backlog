# A writes-overlap serialization (after:) is dropped when the overlapping path leaves either card's writes

Parent: E-0001
severity: S2

The feeder writes `after: <id>` when two cards' `writes:` overlap ("serialized behind T-x: writes: overlaps <path>"). It never drops that line when the overlap goes away: a later card edit removes the path from either card's writes, but the `after:` stays and holds the card, and everything bundled with it, behind an item it no longer conflicts with. Seen 2026-10-07 on botseon: T-0659 kept `after: T-0122` after T-0122's writes no longer held the overlapping test file, and that held a Feature's delivery until it was removed by hand.

## Acceptance
- An `after:` the feeder wrote for a writes overlap records the overlapping path(s) in its History line, or in a structured field.
- On every feeder pass, an `after:` whose recorded paths no longer overlap the two cards' current `writes:` (exact path or glob) is dropped, with a History line "serialization dropped: <path> no longer in <id>'s writes".
- An `after:` set by hand or by a decision (no recorded overlap) is never dropped automatically. When its target is Closed or Retired, `asf next` shows it as "stale serialization".
- Tests: an overlap removed from one card's writes drops the after: in one pass; an overlap still present keeps it; a hand-set after: is kept and flagged when stale.
