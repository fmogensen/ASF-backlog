# Release readiness criterion 10: seats used — no idle-while-launchable stretch over 30 min below 60%

Operator 2026-10-06: "make seat utilisation alarm part of release readiness".

Add criterion 10 to `asf release-readiness`: "Seats used — no idle-while-launchable stretch in the window".
- Red when, in the last `release.window_days` (7), any stretch of >= `release.seats.idle_min` (default 30) minutes had seat utilisation < `release.seats.min_pct` (default 60%) of the available seats (local share + cloud max, quota-allowed accounts only) WHILE launchable rows existed (the feeder's would-launch count > 0) — i.e. capacity wasted, not a quiet factory.
- A quiet factory (no launchable rows) never counts against it.
- Evidence column: number of idle stretches, the longest (start, minutes, busy/available), and the top cause from idle accounting.
Depends on: "Throughput metrics with history" item 1 (seat utilisation stored per tick) — this criterion reads that series.
Acceptance (hermetic): fixture tick streams → red for a 31-min 2/8 stretch with launchable rows; green for the same with zero launchable; green for 29 min; thresholds from config; row printed by `asf release-readiness` and mirrored in `asf doctor`.
Real case: 2026-10-05/06 night, botseon 1–3/8 local, 0/12 cloud for 50+ min with 3 rows launchable.

## Question
This reads as a defect. A Bug carries a signature — add signature: <the failing test or error line>; or an ## Acceptance list if it is new work.
