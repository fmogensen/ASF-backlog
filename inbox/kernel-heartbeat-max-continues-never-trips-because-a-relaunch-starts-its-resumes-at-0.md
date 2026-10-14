# kernel.heartbeat.max_continues never trips because a relaunch starts its resumes at 0
parent: F-0360

Type: Bug

kernel.heartbeat.max_continues never trips: decide.py compares Session.beats, read from the live run's `resumes`, but `resumes` is written only on the ended row in asf/kernel/ports.py `_settle_beat`, and the relaunch is a new job that starts at 0.

## Acceptance
- [ ] A relaunch after a beat stall carries the chain's resumes on its launch row, proved by a test.
- [ ] The max_continues-th beat-stall end of one item's chain makes it Stuck.
