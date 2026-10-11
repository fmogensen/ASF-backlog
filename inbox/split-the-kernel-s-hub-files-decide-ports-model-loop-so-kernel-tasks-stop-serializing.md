# Split the kernel's hub files (decide/ports/model/loop) so kernel Tasks stop serializing
parent: F-0352

type: story
parent: F-0352

Split the kernel's hub files so kernel Tasks stop serializing on them. Measured 2026-10-11 ~20:25Z: every launchable Ready Task (13 of 13) was held by file overlap with the 5 building Tasks. The hot paths were asf/kernel/decide.py (6 holds), asf/kernel/ports.py (6), asf/kernel/model.py (5), asf/kernel/loop.py (4), tests/test_tick*.py (3 each). Almost all 0.3 work changes the kernel, so it runs one Task at a time through these files.

## Change
- Split decide.py into one module per concern (launch/seats, judge/rounds, landing/merge, LIMBO/flow, intake), with decide.py as a thin composer. Split ports.py per port, and model.py's Config into per-block dataclasses. Behaviour unchanged.
- Tests follow the same split (tests/kernel/test_<concern>.py).
- Plans and briefs name the concern module, not decide.py.

## Acceptance
- The kernel suite passes unchanged in count and outcomes before and after the split, with only test-module moves (test).
- On a replay of the measured overlap set, at least half of those 13 Tasks no longer overlap with each other by write path (test over their declared writes, re-pointed to the new modules).
- Measured after it lands: overlap holds per day vs 2026-10-11.
