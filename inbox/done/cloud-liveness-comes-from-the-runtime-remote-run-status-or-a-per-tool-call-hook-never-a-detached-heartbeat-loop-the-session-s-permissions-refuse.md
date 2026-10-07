→ F-0265

# S1: cloud liveness comes from the runtime (remote run status or a per-tool-call hook), never a detached heartbeat loop the session's permissions refuse

Parent: E-0001
severity: S1

Cloud sessions are told to start a detached background heartbeat loop. Their own permission layer refuses it as persistence, so no beat arrives. After ~20 min the run is marked "stalled: no beat", even while it is working. Seen 2026-10-06/07 on botseon (replan-f-0003, T-37354, T-42278, correct-t-44931 "no beat 21m") and on ASF (review-t-0755, coder-t-0496 and others). Cloud capacity is wasted and items fall back to local only.

## Acceptance
- No brief asks a session to start a detached or background process for liveness.
- A cloud run's liveness comes from the runtime, not the session. Either the remote run's own status (the trigger/session API: running, idle or ended, plus its last-event time) is read on every tick as the beat, or a per-tool-call hook in the session settings writes the beat. The hook runs inside the harness, so it needs no background process.
- A cloud run whose remote status is running, with an event in the last N minutes (configurable), is never marked stalled. A run whose remote status ended with no report gets the dead reason from F-0264's refusal capture.
- Local sessions keep their current beat. The stall threshold and the beat source are per executor kind in config.
- Tests use a fake remote API: a running run with fresh events is not stalled; a running run with no events past N is stalled; an ended run is dead with its reason. A brief scan fails the build if any brief text asks for a background loop.
