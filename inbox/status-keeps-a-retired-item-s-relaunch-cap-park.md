# status keeps a retired item's relaunch-cap park

Severity: S2

Reported by botseon on 2026-10-08. `status` still shows B-1557's relaunch-cap park after B-1557 was retired.

## Acceptance
- Retiring, closing or resolving an item clears its park, cap and hold rows from `status` and `next` on the next read. A test covers it: park → retire → no row.
