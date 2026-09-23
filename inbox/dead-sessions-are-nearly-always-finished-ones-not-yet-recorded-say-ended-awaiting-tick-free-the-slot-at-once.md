# 'dead' sessions are nearly always finished ones not yet recorded: say 'ended, awaiting tick', free the slot at once
parent: E-0001

Bug, severity S2, parent E-0001. Decided by the operator on 2026-09-23 ("since dead is misleading it should be fixed").

`asf status` (Agents row) and `asf sessions` (the Dead table) call a session "dead" as soon as its pid is gone and the ledger holds no end record. Nearly every such session has in fact finished normally. The next tick records it as `finished`, and often lands it. On 2026-09-23 every "dead" session checked turned out finished: fix-bug-b-0080, b-0083, b-0069 and coder-t-0052. The operator reads "dead" as lost work, and the slot counts as taken for up to one tick interval.

Expected:
- A session whose process has exited but that no tick has recorded yet shows as **ended, awaiting tick**. The view derives its result where it can: the session's own end-of-run record (F-0089) or its pushed branch.
- "dead" is kept for real deaths: killed, crashed, or no end-of-run record and nothing pushed. Each is shown with the reason.
- The slot an ended session holds is released at once, not at the next tick, so the wave does not wait up to 10 minutes for capacity.
- Tests cover all three cases in the view and in capacity.
