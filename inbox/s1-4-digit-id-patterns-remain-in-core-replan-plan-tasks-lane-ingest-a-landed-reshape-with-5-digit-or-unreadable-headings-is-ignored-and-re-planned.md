# S1: 4-digit id patterns remain in core/replan/plan_tasks/lane/ingest; a landed reshape with 5-digit or unreadable headings is ignored and re-planned

Parent: E-0001
severity: S1

B-0273 fixed id matching in landing, but 4-digit-only id patterns remain across the record. Every newly minted id has 5 digits, so these fail silently:
- asf/record/core.py:41 `ID_RE = ^[A-Z]-\d{4}$` (record id validation);
- asf/record/replan.py:71/73/74/81 (replan header, Task and Drop headings, ids);
- asf/record/replan.py:171 and asf/record/plan_tasks.py:34 (`S-\d{4}` stories);
- asf/harvest/lane.py:248 `ITEM_ID_RE`;
- asf/record/ingest.py:633 `merged into`;
- asf/feeder/rows.py:592 `MERGED_INTO_RE`.
Also, a landed reshape plan that the replan reader cannot apply is silently ignored, and the item goes back to RESHAPE → PLAN, so the factory plans it again. Seen 2026-10-07 on botseon: T-0660's reshape (#1230, merged 12:48) wrote docs/superpowers/plans/f-1144.md "## 4. The Tasks — reshaped from T-0660" with `### T-49891: …` headings. These are 5-digit ids, in a Feature plan file rather than plans/replans/ with a `replan:` header. Nothing minted, T-0660 kept `reshape: split …`, and status offered a fresh RESHAPE.

## Acceptance
- One shared id pattern (`[A-Z]-\d{4,}`) is used by every record, feeder, harvest and evidence parser. A repo-wide test fails if any `\d{4}\b` id regex outside date or time parsing remains. Tests use 5-digit fixtures for core id validation, replan headings, `stories:`, lane item ids and merged-into.
- The reshape brief tells the session the exact readable output: a file under plans/replans/ with a `replan:` header and `### Task new N:` / `### Task <id>:` / `### Drop <id>:` headings. Or the applier also reads a Feature plan's "reshaped from <id>" section. Tested both ways.
- When a reshape or replan for an item has landed but cannot be applied, the item gets an INPUT or doctor row ("reshape landed but unreadable: <why>"). It never gets a fresh RESHAPE launch. Tested.

## Question
This reads as a defect. A Bug carries a signature — add signature: <the failing test or error line>, or paste that line into the body (an `Error:` line or a `file:line › test` line is read as one); or an ## Acceptance list if it is new work.

## Two snags from the hand-applied reshape (botseon, 2026-10-07 13:47)
- [ ] `asf new` leaves an uncommitted card file behind when the pre-commit check refuses the commit (orphan T-51301). On refusal it removes the card and reverts index.json and the parent link lines, leaving the tree clean. Tested.
- [ ] Plan and reshape bodies cite decision-register entries as bare `D165`/`D43`, and the record check blocks those as malformed ids. The check accepts `D<n>` that resolves to an existing docs/decisions entry, or the applier rewrites it to the record's id form, so a reshape that cites decisions applies cleanly. Tested.
