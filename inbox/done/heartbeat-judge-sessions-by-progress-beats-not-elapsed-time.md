→ F-0347

# Heartbeat: judge sessions by progress beats, not elapsed time
parent: E-0003

Heartbeat: judge running sessions by evidence of progress, not elapsed time (operator-approved 2026-10-10, ASF 0.3). Heartbeat, not keep-alive: every running thing beats; the kernel judges beats.

## Problem
Today the kernel ends a session only after a time bound (class p90, x2 once pushed, else a fixed knob): 110 such kills in the kernel log, e.g. a spec session after 36 min. 24 cloud runs ended "succeeded" with no report commit and were only discovered at run end. Meanwhile asf/progress.py already writes per-session progress (calls, novel calls, last_call_at, a call ring) every minute and the kernel never reads it.

## Change (this Feature: local sessions + first sign of life + the kernel's own beat)
- Kernel reads the progress beat of each live local session. Frozen: no tool call for `kernel.heartbeat.frozen_after_s` (default from the measured inter-call gap p99, seed 600) -> end and relaunch on its branch the same tick with "stalled at <last step>" as a finding. Looping: no novel call in the last `kernel.heartbeat.loop_window` calls (seed 20) -> end and relaunch on the strong model with the loop as a finding.
- First sign of life: a session (local or cloud) with no beat and no push within `kernel.heartbeat.first_beat_s` (seed 600) -> relaunch.
- The kernel tick writes its own beat; `asf kernel watch` judges that beat (missed beats -> kick), replacing the age-of-log keep-alive check.
- Builder briefs require a WIP commit+push at each green step so a relaunch resumes rather than restarts.
- Time bounds stay only as last resort.
- Cloud heartbeat via `refs/asf/hb/<job>` is a later stage, built with F-0344 Stage 2 (refs/asf/log).

## Acceptance
- A fixture session with no tool call past the frozen bound is ended and relaunched with the finding on that tick (test).
- A fixture session whose last N calls are all repeats is relaunched on the strong model (test).
- A session with no beat and no push past first_beat_s is relaunched (test); a cloud run that ends with no report commit is caught by that rule before run end on a fixture (test).
- `asf kernel watch` kicks a kernel whose beat is stale and leaves a beating one alone (test).
- A healthy fixture session making novel calls is never ended before the last-resort time bound (test).
