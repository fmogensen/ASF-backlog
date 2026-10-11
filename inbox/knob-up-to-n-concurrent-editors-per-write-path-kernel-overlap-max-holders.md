# Knob: up to N concurrent editors per write path (kernel.overlap.max_holders)
parent: F-0352

type: story
parent: F-0352
severity: S1

First Story of F-0352: a knob for concurrent editors per write path. Today asf/kernel/decide.py `_overlap()` allows exactly one Building item per overlapping path (no config). Measured 2026-10-11 ~20:25Z: 13 of 13 launchable Ready Tasks were held by overlap on the kernel hub files.

## Change (small, one Task)
- `kernel.overlap.max_holders` (default 1 = today). With N > 1, up to N Building items may share a write path. A launch that shares a path gets brief lines: keep edits additive, merge origin/main before each push, resolve conflicts in your own round.
- A merge conflict on such a PR becomes a fix round with the conflict as the finding (existing path), never a Stuck.
- Set `kernel.overlap.max_holders: 2` in ASF's own product yaml once landed.

## Acceptance
- With max_holders 2, two Ready Tasks sharing decide.py both launch while a third waits (test); with 1, behaviour is unchanged (test).
- Measured after install: overlap holds per day vs 2026-10-11, and merges per hour.
