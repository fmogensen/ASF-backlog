# plan-tasks treats a foreign id cited in plan prose as a mint and refuses the whole plan
parent: F-0288

Severity: S2

`plan-tasks` refused F-0288's whole plan: "mints id(s) no claim covers: B-1377 is outside F-0288's claimed block". The plan never minted B-1377. It cited another product's bug in prose (docs/plans/f-0288.md:34 and :100). `idcheck.check_doc` (asf/record/plan_tasks.py:185) treats every id-shaped token that ASF's record lacks as a mint, so a plan citing any foreign or historical id is refused and its Feature stalls with no Tasks. Worked around for F-0288 by rewording (PR #1204); the checker is unchanged.

## Acceptance
- A plan whose prose (outside `### Task N:` / `### S-…:` headings and their declared id fields) mentions an id that is not in the record and not in the claim is minted. A test pins this with the F-0288 shape: a Task table plus a prose line citing `B-1377`.
- An id that a Task or Story heading actually declares outside the claimed block is still refused by name (the existing refusal test stays green).
- A mention of an id that the record holds for another card, inside a Task body, still refuses as today.

## Question
A Story is one PR with one acceptance list — add an ## Acceptance checklist.
