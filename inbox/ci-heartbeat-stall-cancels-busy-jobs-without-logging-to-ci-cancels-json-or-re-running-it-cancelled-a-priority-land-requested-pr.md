# ci-heartbeat stall-cancels busy jobs without logging to ci-cancels.json or re-running; it cancelled a priority land-requested PR

Parent: E-0001
severity: S2

The CI-box stall watchdog (ci-heartbeat, mode=act) cancels a running job's whole run without recording it in ci-cancels.json and without re-running it. A land-requested PR then sits on a cancelled run. Seen 2026-10-07 21:33 local on botseon: `STALL runner=nordio-ci-8 job=gate-tests run=37670494471 elapsed=1206s silence=492s max=1183s cpu=18.3` → `ACT cancel run=37670494471`. That was PR #1070, a priority-land-requested goal PR, and every heavy job was cancelled. The job was using CPU (18%), so it wasn't hung, only past a max of 1183 s, which is barely above its normal run time. The cancel appeared only in ~/.ASF/logs/ci-heartbeat.log.

## Acceptance
- Every cancel ASF makes, ci-heartbeat stall acts included, writes a ci-cancels.json entry with the cause (`stall`), the job, the runner, elapsed, silence and cpu, so `asf ci cancels` and the land-request line show it. Tested.
- A stall-cancelled run on a PR's current head is re-run once automatically (head-matched). A second stall on the same head breaches the watchdog and is not cancelled again. Tested.
- The stall rule acts only when the job is both silent and idle (cpu below a configurable floor). A job past its max but still busy gets a warning line, not a cancel. The max comes from the job's measured p95 baseline times a configurable factor, with a floor. Tested.
- For a land-requested or priority PR, the stall act defers to a warning unless silence exceeds a larger configurable limit. Tested.

## Interim (2026-10-07 21:5x)
The max table is the hand-kept ~/.ASF/state/ci-heartbeat/job-max.json, built from baselines of 10-01..04 that nothing refreshes. gate-tests (hetzner) was raised by hand from 1183 s to 1800 s; a backup sits beside it. Acceptance adds:
- [ ] job-max is derived from the live runner baseline (asf ci baseline, p95 times a factor) and refreshed daily. No hand-kept table. Tested.
