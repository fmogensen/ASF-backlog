→ F-0329

# minting a declared Story (idcheck.declared_stories / plan_tasks) strips backtick code spans from its acceptance lines
parent: E-0001
severity: S2


Minting a declared Story (`idcheck.declared_stories` / `plan_tasks`) strips backtick code spans from its acceptance lines: S-73204..06 were minted from docs/specs/f-0280.md with every `code` term gone ("a Feature at  ,   in lane   round 1"), so the cards no longer state what must be proved. They were restored by hand.

## Acceptance
- [ ] a Story minted from a declaration keeps each acceptance line byte-for-byte, code spans included: a test with a backticked line, through `declared_stories` and `plan_tasks`
- [ ] `declared_stories` still ignores `### S-…` headings inside fenced code blocks
