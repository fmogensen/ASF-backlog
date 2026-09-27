→ F-0206

# Orphaned dead session row (B-1381, unknown account) never reaped for 5 h
parent: E-0001

botseon `asf sessions`/status have shown one dead row "B-1381 | fix | <unknown account> | dead pid" since ~18:30 on 2026-09-26 (5+ h, many harvests). B-1381's fix was pushed and is at PR #855 (later sessions ran code/review on it), so this ledger row is orphaned: health/harvest never ends it. Its account is not one of this host's configured worker accounts — likely a row written by another launcher. Fix: health ends any session row whose pid is dead (or whose account is unknown to this host) once its item has a newer session or its branch moved past the row's launch head; log "ended <job>: dead pid, superseded by <job>". Test: orphan dead row with a newer session → ended on next health pass.
