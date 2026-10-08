→ F-0325

# a declared 'wave' clock with steps [wave] runs as a tick under the tick lock and suppresses the native wave job
parent: E-0001
severity: S2


A product that declares its own clock named `wave` with `steps: [wave]` (as botseon.yaml:300 did, set up for ASF #54 before the native job existed) gets `asf.cli tick --steps wave`. That is a tick under the product's tick lock, and it suppresses the native wave job (asf/tick/wave_clock.py, `asf wave`). On 2026-10-08 botseon's wave log repeated "another tick of botseon is running — skipped" 1,270 times. During a 10-minute tick it had 6 rows ready and cloud at 2/32, so launches only happened between ticks.

## Acceptance
- A declared clock named `wave` whose steps are exactly `[wave]` is installed as the native `asf wave` job (its own lock and snapshot), not as a tick. Either the scheduler maps it, or the config check refuses it with the fix named. A test covers it.
- `asf doctor` prints one red row when a product's wave job runs under the tick lock, naming the yaml line to remove.
