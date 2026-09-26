# I5-violating rows show as Ready (first) but are silently never launched (botseon T-0038 since 19:00)

botseon T-0038 "FIX → CORRECT" (cloud/tinkerer-mode-t1) has been FIRST in Ready at most statuses since ~19:00 on 2026-09-26, but no wave ever launched it and no `waits` line names it; every tick logs "INVARIANT I5: FIX → CORRECT T-0038 @cloud/tinkerer-mode-t1 — F-0092's spec and plan not on the trunk". So the row is dropped by the I5 check silently while status/next keep showing it as ready-to-launch first, misleading both sessions (and 3f4b0b9's "every unlaunched row says why" doesn't cover it).

Fix: a row that violates I5 is not Ready — status/next show it as "WAITS ON F-0092 spec+plan on the trunk" (tier kept), and the wave logs that reason; optionally route the parent Feature's spec/plan landing as the actionable row. Test: I5-violating correct row appears as WAITS with the reason, never as Ready/first.

## Question
Which Epic is this under? No open Epic shares a title word with it.
