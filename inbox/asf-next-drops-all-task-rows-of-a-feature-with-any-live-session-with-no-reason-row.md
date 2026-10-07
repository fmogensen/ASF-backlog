# asf next drops all Task rows of a Feature with any live session, with no reason row

Parent: E-0001
severity: S2

`asf next` drops every Task row of a Feature that has any live session, and gives no reason. In asf/feeder/rows.py `_one_feature_rows` (around lines 1503-1508), `if fid in busy:` returns without Task rows unless the Feature has no Stories (`needs_stories`). A Feature with Stories and a running or needs-input replan, spec-amend or correction session therefore loses every open Task row. `busy` counts any session whose item is the Feature (`inflight_ids`). A Task that showed "WAITS ON X" turns invisible when X closes. Seen 2026-10-07 on botseon: F-0003's rank-1 decided Task T-37153 had no row while replan-f-0003 and its correction sat in needs-input, and nothing said why.

## Acceptance
- With a live session on its Feature (replan, spec-amend, correction), every open, decided Task still gets a row in `asf next --all`. The row either launches, when the session doesn't touch Task work (a correction on another item), or reads `WAITS ON <session kind> <fid> (<job>, <state>)`.
- A Feature session in needs-input for more than N minutes (configurable) is named on its waiting Tasks' rows as the holder, with its input question.
- `asf next --all` lists a row for every open, decided item, or prints why it is excluded. No silent omission; a test asserts it over a fixture record.
- Test: a Feature with Stories and one in-flight `replan` session. `feature_rows` returns a WAITS row for each open Task, and none launches.
