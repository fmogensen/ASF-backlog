# Pending upgrade starves: every owner tick defers because another owner tick is running

The pending upgrade to 5cab3a2 sat for over 16 minutes while botseon ticks printed "tick: waiting — upgrade to 5cab3a2 pending". Main was green throughout.

Cause: every owner (asf) tick that started logged "upgrade: deferred to the next tick — another asf tick is running (pid N)". asf ticks overlap, since a tick with a harvest outlives the 5-minute clock, so the owner never found a gap. Meanwhile the other product's ticks were parked. The operator's session installed it by hand at 06:41.

Expected: the pending upgrade installs within one tick interval once CI is green. For example:
- the owner tick waits (bounded) for the running tick to finish, then installs; or
- the install runs from whichever tick finishes last; or
- the parked product's tick does the install itself when the running asf tick holds no lock the install needs.

Test: overlapping owner ticks → the pending upgrade installs within N passes. It must never be "deferred" forever.

## Question
Which Epic is this under? No open Epic shares a title word with it.
