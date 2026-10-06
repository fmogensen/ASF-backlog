# S1/S2 inbox cards never wait on an intake question; intake decides Epic and kind by rule

Parent: E-0001

S1 inbox cards wait hours on intake questions ("Which Epic is this under?", "add a signature"). On 2026-10-06, two S1 cards and four others sat 2–3 h in ASF's inbox until someone answered `→ answer: ____` lines in groom/<day>.md by hand. The groom also read the untouched placeholder as an answer: "answer not applied — inbox:item-ids-…md: "____" is not a clause". The record-side rule is "questions to the operator are a bug": the intake can decide these from facts it already has.

## Acceptance
- A card whose title or body starts with `S1`/`S2`, or carries `severity: S1|S2`, never waits on an Epic question. It is filed under the product's configured default Epic (`groom.default_epic`, falling back to the oldest Active Epic), and the History line says so.
- A card with an `## Acceptance` section is never asked for a bug signature. It becomes a Feature or Task, or a Bug whose signature is its title.
- A `____` placeholder is never parsed as an answer: no "is not a clause" error, and the line stays open.
- An intake question still unanswered after N minutes (configurable, default 30) is decided by the rules above, recorded as "decided by rule", and reversible by an answer.
- Tests cover each line.
