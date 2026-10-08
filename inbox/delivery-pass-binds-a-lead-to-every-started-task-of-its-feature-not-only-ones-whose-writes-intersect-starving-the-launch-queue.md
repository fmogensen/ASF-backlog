# delivery pass binds a lead to every started Task of its Feature, not only ones whose writes intersect, starving the launch queue

Severity: S1

Reported by botseon on 2026-10-08 (0.1.245–0.1.260). The tick's delivery pass, `asf.record.slice.deliveries` (asf/record/slice.py:226-233), makes every new delivery lead `after:` each started Task of its Feature that no member already waits on. It never checks whether their `writes:` intersect. On botseon this added about 43 edges across F-0124, F-0115, F-0111 and other Features. Launchable parity rows fell to 0 of 278 while 23 of 32 cloud seats sat idle. Botseon removed the edges by hand, and 9 sessions launched within one wave.

A lead already in a delivery is skipped by `free_tasks`, via `_in_delivery`, so a removed edge is not re-added. Every delivery formed from now on still gets the extra edges.

## Acceptance
- A delivery lead gains an `after:` on a started Task only when that Task's `writes:` intersect a member's `writes:` (by `asf.record.core.writes_intersect`), or when a member already declares `after:` on it. A test pins it: Feature with a two-Task bundle and one started Task with disjoint writes → the lead's `after:` is unchanged.
- A started Task whose `writes:` overlap a member's still binds the lead (the existing test stays green).
- An `after:` edge removed from a lead that is already in a delivery is not re-added by the next tick. A test pins it: run the pass twice and remove the edge in between.
- The pass prints one line naming each edge it adds and the shared path that caused it.
