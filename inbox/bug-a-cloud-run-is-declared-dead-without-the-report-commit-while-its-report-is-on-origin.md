# Bug: a cloud run is declared dead without the report commit while its report is on origin

Bug (defect): a cloud run is declared dead "without the report commit" while its report is on origin

Type: bug. A fix exists on an open PR (fix branch for T-81130, which is a framework fix, not T-81130's delivery); this card owns that fix so the kernel never mistakes it for T-81130's own PR.

## Defect

Live 2026-10-10: 26 claude-remote runs were declared dead "without the report commit"; 11 of them had their report commit on origin.
- `report_commit` read only the last trailer paragraph; a trailing blank line plus a `Signed-off-by` paragraph hid `ASF-Session`/`ASF-Report`.
- A routine's `last_run` reads SUCCEEDED when its first turn ends; sessions pushed their report 17-226 min later, after the dead verdict.

## Fix

- Trailers are read across blank lines.
- An ended run with no report is held while its session is `running` or had an event within `cloud.report_grace_min` (default 45; 0 = old verdict). A dead verdict is read only from a fresh session answer; `timeout_min` stays the backstop. A late report's FINISHED line records its delay.

## Acceptance (tests)

- tests/test_remote.py::AnEndedRunWaitsForItsReport (replays a real report body with a trailing Signed-off-by paragraph; an ended run with a live session is held; a dead verdict needs a fresh session answer).

## Question
Which Epic is this under? No open Epic shares a title word with it.
