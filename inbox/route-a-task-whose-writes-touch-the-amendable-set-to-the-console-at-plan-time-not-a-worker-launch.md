# Route a Task whose writes: touch the amendable set to the console at plan time, not a worker launch

A Task whose `writes:` footprint touches the amendable set is launched as a worker session anyway,
and the only check is the run-time hook refusal. T-0183 (F-0093) declared
`asf/briefs/templates/groom.md` and `groom-clerk.md` in `writes:` from the day it was minted
(2026-09-24); it was launched, refused `touch_amendable_set` five times since 08:30Z on
2026-09-27, each refusal a spent session and a `NEEDS OPERATOR`, until the console landed it by
hand (PR #121, with T-0187). The footprint said so at plan time; nothing read it.

Where the check is today (origin/main at 3465cf21d):
- run time: `asf/approvals.py:881` — `run_hook` calls `amendable.write_target` and refuses
  `touch_amendable_set` (`asf/approvals.py:884`); `asf/amendable.py:102` `write_target`,
  `asf/amendable.py:95` `_in_set`, `asf/amendable.py:62` `paths`.
- land time: `asf/approvals.py:448` `merge_class` → `merge_amendable_set`, read by the harvest at
  `asf/harvest/lane.py:2375`.
- the park it buys: `asf/approvals.py:98-101` (`touch_amendable_set`, `parks=True`) and
  `asf/approvals.py:1049` `parked` — a park that `asf approvals resolve … dropped/done` lifts, so
  the next tick relaunches into the same refusal.

Where it could be seen earlier (the footprint is already in hand):
- `asf/feeder/rows.py:884` reads `writes = t.get('writes')` for every PLAN → CODE candidate; the
  launching row is minted at `asf/feeder/rows.py:904`. A `writes:` glob that `amendable._in_set`
  matches (glob-vs-glob, as `amendable._reach` at `asf/amendable.py:136` already does) could turn
  the row into a non-launching `WAITS ON console: amendable <path>` (`waits_on='operator'`), the
  same shape `hold_classes` (`asf/feeder/rows.py:1024`) uses — no session, no slot, still shown in
  `asf next`/status, and never a hold that a resolve silently undoes.
- `asf/record/plan_tasks.py:62` `mint_plan_tasks` — could flag it once at mint time (an
  `amendable:` line on the card, or a planner rule that splits the amendable hunk into its own
  Task) so the plan reviewer sees it.
- the same test belongs on correction/review/rebase rows for that item (`correction_rows`,
  `lane_rows` in `asf/feeder/rows.py`), since T-0183's last refusals came from a REVIEW relaunch.

Wanted: a Task whose declared footprint intersects the amendable set is routed at plan time to a
console lane (or flagged and held with one operator line), never launched as a worker session;
test: a `writes:` naming `asf/briefs/templates/x.md` yields no launching row. Not implemented here.

## Question
Route to a console lane (the console lands it, as PR #121 did) or split the amendable hunk into a
separate console Task at plan time?
