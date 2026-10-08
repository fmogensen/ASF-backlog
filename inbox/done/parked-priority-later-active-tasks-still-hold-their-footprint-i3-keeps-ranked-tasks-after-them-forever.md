→ B-82960

# parked (priority later) Active Tasks still hold their footprint; I3 keeps ranked Tasks after: them forever
signature: writes: intersects Active task of a priority later Feature
parent: E-0001
severity: S1


Since the 2026-10-08 scope freeze (about 178 Features set to `priority: later`), their Tasks that were already Active still hold their `writes:` footprints. Every ranked Task touching the same hot files (`asf/cli.py`, `asf/conventions.py`, `asf/feeder/rows.py`, `asf/record/core.py`) carries `after:` edges to them. Invariant I3 refuses removing those edges ("writes: intersects Active task …"), so the ranked Tasks can never launch:
- T-79565 (F-0301, rank 5) waits on 21 parked Tasks;
- T-80572 (F-0306, rank 2) waits on 7;
- T-81129 (F-0312, rank 4) waits on T-79565;
- 16 more need/unranked Tasks are blocked the same way.

The feeder shows them as "WAITS ON T-0056 (parked: F-0023 later)". At this check: 0 of 8 cloud seats busy and 1 row ready to launch. A factory fix for this is itself blocked, because its Task writes the same files.

## Acceptance
- An Active Task whose Feature is `priority: later`, or whose row is parked, holds no footprint. I3 and the feeder's footprint gate skip it, and `after:` edges to it are dropped from Tasks of non-later Features (with a History line naming the parked holder). A test covers it: a need Task with `after: [T-p]`, where T-p is Active under a later Feature with intersecting writes → the need Task draws a launching row, and `asf set … after-=T-p` is accepted.
- When a later Feature is raised again, its Active Tasks get their `after:` order back behind whatever landed meanwhile and rebase before they build. A test covers it.
- `asf next` names a ranked Task held only by later-priority holders as such, so the stall is visible in one line.
