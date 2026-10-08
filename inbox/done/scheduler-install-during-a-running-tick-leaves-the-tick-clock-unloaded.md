→ F-0331

# scheduler install during a running tick leaves the tick clock unloaded
parent: E-0001
severity: S2


Reported by botseon on 2026-10-08. `scheduler install` ran at about 18:55Z while a tick was running (pid 47188, started 18:37Z). It left the product's tick clock "not loaded", and the install output listed only batch, daily, ci-queue and wave. A second install at 19:00Z loaded it. In the meantime the product had no tick clock and nothing said so.

## Acceptance
- `scheduler install` ends with every declared clock loaded. A clock whose job is running is re-bootstrapped once the running process ends, or is loaded without killing it, and the output names every clock with its state. A test covers it: install while the tick lock is held → the tick clock reports loaded (or "loads when pid N ends") and is loaded afterwards.
- `asf doctor` shows one red row when a declared clock is not loaded.
