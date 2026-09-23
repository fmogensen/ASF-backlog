→ F-0098

# 'dead' sessions are nearly always finished ones not yet recorded: say 'ended, awaiting tick', free the slot at once
parent: E-0001

Bug, severity S2, parent E-0001. Decided by the operator on 2026-09-23 ("since dead is misleading it should be fixed").

`asf status` (Agents row) and `asf sessions` (the Dead table) call a session "dead" as soon as its pid is gone and the ledger holds no end record. Nearly every such session has in fact finished normally. The next tick records it as `finished`, and often lands it. On 2026-09-23 every "dead" session checked turned out finished: fix-bug-b-0080, b-0083, b-0069 and coder-t-0052. The operator reads "dead" as lost work, and the slot counts as taken for up to one tick interval.

Expected:
- A session whose process has exited but that no tick has recorded yet shows as **ended, awaiting tick**. The view derives its result where it can: the session's own end-of-run record (F-0089) or its pushed branch.
- "dead" is kept for real deaths: killed, crashed, or no end-of-run record and nothing pushed. Each is shown with the reason.
- The slot an ended session holds is released at once, not at the next tick, so the wave does not wait up to 10 minutes for capacity.
- Tests cover all three cases in the view and in capacity.

Operator, 2026-09-23: fix it **systemically**, not as a relabel. The class of defect is that each reader derives session state on its own: `asf status` from the process table, `asf sessions` from the registry plus pids, capacity/the wave from the pool, and the tick from the ledger when it next runs. So they disagree in the gap between ticks, and every view can mislead the same way again with the next state that gets added.

Expected, systemically:
1. **One classifier.** One function in asf.workers.lifecycle maps (ledger records, registry, pid liveness, the session's own end-of-run record, the branch on origin) to one state per run: working · ended-awaiting-tick(result) · finished · failed(reason) · dead(reason). status, sessions, capacity, the wave, harvest and the tick digest all call it, and nothing else derives state. A test asserts that each of those readers imports it and has no pid or ledger logic of its own.
2. **The session writes its own end.** With F-0089, the session appends its end record to the ledger as it exits, so "awaiting tick" becomes a window of seconds, not up to 10 minutes. The tick reconciles; it does not discover.
3. **Capacity counts only working runs.** An ended run frees its slot the moment its end record is written.
4. **An invariant test** (F-0087 style) for the class: for any sequence of ledger and pid events, every reader reports the same state for the same run.
