# The record pre-commit hook should rebuild and stage a stale index.json instead of refusing

The record pre-commit's only blocking error on a hand edit was "index.json is stale (run `asf index`)", caused by the commit's own staged edit (botseon, 2026-09-25). Have the hook regenerate index.json and stage it itself, as asf set already does, instead of refusing, so a hand edit doesn't need the extra step. It should still refuse any real record error.
