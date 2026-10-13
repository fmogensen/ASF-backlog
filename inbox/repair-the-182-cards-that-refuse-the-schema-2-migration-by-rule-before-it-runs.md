# Repair the 182 cards that refuse the schema-2 migration by rule, before it runs
parent: F-0340

Type: Bug
Priority: need

## Description
182 of 2,913 cards of this product's own record refuse the schema-2 migration (measured 2026-10-13 with the per-card path of PR #1684 on a copy of the record). The migration is right to refuse; the cards are malformed. All refusals but nine are mechanical, and none of the nine is open work, so no LLM fill is needed: a deterministic `asf record-repair` pass clears them before the migration runs.

| reason | cards | open | rule |
| --- | --- | --- | --- |
| body: unbalanced code fence | 110 | 23 | close the fence at the end of the body |
| body: duplicate section heading (`Description`) | 25 | 9 | merge the sections; drop an empty duplicate |
| acceptance: not a checkbox bullet | 28 | 1 | a bare `- text` becomes `- [ ] text`; 9 done cards carry free prose: wrap as one unchecked entry |
| body: text before the first section | 9 | 0 | move a stray `key: value` line into the frontmatter or `extra` |
| card: rank below 0 | 6 | 0 | drop the key |
| history: not a bullet | 4 | 2 | fold a continuation line into the previous bullet |

45 of the 182 are removed cards and can be archived unchanged (see F-0363).

## Acceptance
- [ ] `asf record-repair` gains these six rules, each with a test on a card of that shape, a dry run that lists every card and rule, and one commit per rule on a real run.
- [ ] After the repair, the per-card migration path refuses 0 cards of the working set on a copy of a real record of this size.
- [ ] No rule changes a card that already converts; a repaired card round-trips byte for byte on a second run.
