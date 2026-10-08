# S1 regression: F-0282 staged-guard refuses groom's inbox→done move, so no answered inbox card can be minted

Parent: E-0001
severity: S1

A regression from the F-0282 staged-guard (hardening pass, 0.1.243): `asf groom --apply` can't land an answered inbox card. Groom's apply_answer edits the card's header lines, then move_to_done writes it to `inbox/done/` with a new header. The guard's moved check (asf/record/staged_guard.py ~118-120) needs the deleted file's HEAD text to appear verbatim in a written file, which the edit breaks. The commit is refused as "is on origin/main and this commit deletes it", so no answered inbox card is ever minted. Seen 2026-10-08 on botseon: 4 answered cards were stuck (record 2bfcf6f84).

## Acceptance
- A deletion of `<intake>/<name>.md` paired, in the same commit, with an add of `<intake>/done/<name>.md` is always allowed: a path-paired move, whatever the text. Tested with groom's real answer-then-move sequence.
- Record writes made by asf itself (groom, publish) pass the guard by declaring their own moves, with no environment variable. A stale-tree deletion of an unrelated card is still refused. Tested.
