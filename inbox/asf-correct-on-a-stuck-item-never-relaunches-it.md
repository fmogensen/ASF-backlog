# asf correct on a Stuck item never relaunches it
parent: F-0346

Type: Bug

`asf correct <item> --why` on an item that is Stuck after 3 fix rounds records the correction but never relaunches the item. On 2026-10-12, T-159854 got its correction at 08:05Z and was still Stuck at 08:15Z. An `asf answer` on the same item re-judged it within one tick ("escalated: one more fix round on the strong model").

Acceptance:
- A correction on a Stuck item re-judges it on the next tick, the same way an answer does.
- Test: a replay with a Stuck(3 rounds) item plus a correction produces a launch on the next tick.
