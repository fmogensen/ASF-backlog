→ F-0292

# Port the old ci-heartbeat watchdog's OOM re-run into asf ci stall-watch so the host script can be retired

Parent: E-0001

`asf ci stall-watch` (asf/ci_stall.py, F-0286) replaces the host script ~/.ASF/bin/ci-heartbeat-watchdog.py, but it doesn't port the script's OOM half. The old script watches box OOM kills, maps each to a runner slot and job, and re-runs the failed run once (oom-mode act). Until this is ported, the operator can't unload asf.host.ci-heartbeat without losing OOM recovery, so two watchdogs stay installed.

## Acceptance
- `asf ci stall-watch` also reads box OOM kills since its last pass, maps each to runner and job, and, once the run completes failed, re-runs it once (claimed `ci_queue.claim_cancel('oom')`, cause added to ci_cancels.CAUSES). Tested with fixture box output.
- Once ported, `asf doctor` says "old ci-heartbeat watchdog still loaded: unload it". Tested.
