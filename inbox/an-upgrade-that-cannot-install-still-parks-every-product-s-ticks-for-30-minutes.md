# An upgrade that cannot install still parks every product's ticks for 30 minutes

2026-09-25, 21:44–22:10Z: every tick of a product skipped with "tick: waiting — upgrade to f5aa236 pending". No upgrader was running, and main's head was red (2903711), which auto-upgrade refuses to install. The pending marker held the whole factory until its 30-minute expiry. 0 sessions ran, and 8 were ready.

Wanted, in code:
(1) Write the pending marker only for a target the upgrade will actually install: green CI, and no refusal. Clear it the moment the upgrade refuses or the head moves to an uninstallable sha.
(2) The pending expiry becomes 10 minutes, not 30.
(3) The doctor's SCHEDULER row and the status Cron row say "waiting on upgrade to <sha> since <time> (owner <product>)" while a marker holds ticks, never "ok".
