→ F-0317

# landed_doc treats a replan branch's landing-gate correction as landed because the original plan is on main; the Feature deadlocks with no row

Severity: S1

Reported by botseon on 2026-10-08. `landed_doc()` (asf/feeder/rows.py:927-936, called at :988) drops a landing-gate correction on any `plan` branch once the item's evidence holds "plan on origin/main". A **replan** branch is also a `plan` branch kind. Botseon's F-0002 has its original plan on main, so the landing-gate correction on its replan branch (cloud/plan-F-0002-replan, PR #1165, BACK since 2026-10-07 11:12Z) reads as already landed. No row launches it, `next --all` has no F-0002 row, and the tick has printed "WAITS ON landing: … BACK" about 960 times.

Botseon also reported that the replan brief left out S-0138's acceptance lines, and the session ended "needs input — S-0138 has no acceptance text".

## Acceptance
- A landing-gate correction counts as answered by the landing only when **that branch's own** document (the same path and the branch's head content) is on the trunk. A test pins it: original plan on the trunk plus a correction on `plan/F-x-replan` → a FIX → CORRECT row for F-x.
- The F-0090 case stays green: a settled spec or plan hold whose own document landed gets no row.
- A Feature whose correction is skipped by this rule still gets its next-stage row (no Feature with an open lane PR is left with zero rows). A test pins that.
- The replan brief carries every Story's acceptance lines from the record. A test covers a Story with acceptance lines, which appear in the brief.
