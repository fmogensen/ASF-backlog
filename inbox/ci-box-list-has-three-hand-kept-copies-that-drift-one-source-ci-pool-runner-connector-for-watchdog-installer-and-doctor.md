# CI box list has three hand-kept copies that drift: one source (ci.pool / runner connector) for watchdog, installer and doctor

The CI box list exists in at least three hand-kept copies that drift: the product's `ci.pool`, the heartbeat watchdog's BOXES map, and the heartbeat fleet installer's BOXES line. On 2026-10-06 the installer's copy omitted a box rebuilt on 09-20, so that box ran 2+ weeks with no heartbeat and the watchdog alarmed every minute from 23:49 until a hand install at 01:34.
Fix: one source of truth — the product's `ci.pool` (or the CI connector's runner-provider listing) — read by the watchdog, the installer and `asf ci doctor`; the installer installs on every box in the pool that lacks the heartbeat (no wait-for-idle needed: timer + read-only script never touch the runner); `asf doctor` red when a pool box has no heartbeat for > 10 min.
Acceptance: fixture pool of N boxes with one missing its heartbeat → doctor red naming it, installer targets exactly it; no host names or IPs in ASF code (they live only in operator config).

## Question
This reads as a defect. A Bug carries a signature — add signature: <the failing test or error line>; or an ## Acceptance list if it is new work.
