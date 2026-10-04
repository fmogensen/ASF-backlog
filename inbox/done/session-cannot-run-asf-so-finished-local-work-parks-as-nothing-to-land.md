→ F-0238

# Session cannot run asf, so finished local work parks as nothing-to-land

Seen 2026-10-04 on botseon T-0349: the session made the approved transplant 535fd3f9b but reported "this session's sandbox denies every asf invocation outright", so it could not publish and the item parked "nothing to land". Operator published it by hand. Fix: when a session ends with unpushed local commits on its lane branch, the lane publishes them itself (with the usual checks), never parks as nothing-to-land.
