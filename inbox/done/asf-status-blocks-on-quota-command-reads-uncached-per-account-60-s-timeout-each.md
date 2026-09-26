→ F-0167

# asf status blocks on quota_command: reads uncached per account, 60 s timeout each

2026-09-27 01:37: `asf status` hung > 150 s for every product. Stack (faulthandler): views/status.capacity_cell → capacity.resolve → fair_share → usable_slots → pool.band → pool.usage → workers/quota.read → subprocess (quota_command per account, timeout 60 s). The operator quota adapter took 10 s per call because its usage cache stopped refreshing (every read "stale" → a refresh), and status reads quota for every account several times (per product, per share computation). Hand fix: the adapter now refreshes at most once per 5 min.

ASF fix: memoize quota reads per process (and a short on-disk cache, e.g. 60 s, shared by status/next/tick/ci-queue), cap the per-call timeout (e.g. 15 s) and the total quota budget per command; status should render "quota: stale (last read HH:MM)" rather than block. Test: N accounts × M calls → one subprocess per account per process; a slow command doesn't make status exceed its budget.
