→ F-0261

# A job's claimed BACKLOG_ID_RANGE for Story ids goes exhausted before use once the record's id tip passes its hi
parent: E-0001

## Symptom

`asf/record/ids.py:mint_id`, inside a matched `BACKLOG_ID_RANGE`:

```
lo, hi = rng
n = max(lo, max_n + 1) if max_n >= lo else lo
if n > hi:
    raise SystemExit(f"new: id range {prefix}:{lo:04d}-{hi:04d} exhausted")
```

`max_n` is the record's *live* highest existing id of that type, read fresh at call time — not
the id tip at the moment the block was claimed. A block is a create-only ref
(`refs/asf/ids/<P>-<lo>`) pushed before launch, which stops two holders of the *same* block from
colliding, but it does nothing to keep the block valid once *other, later-claimed* blocks resolve
first and push the live tip past this block's own `hi`. A session that is handed a block and then
spends any real time before minting (reading the record, deriving what to mint, investigating a
stalemate) races every other job minting the same type in the meantime — and loses the whole
block, not just the overlap, because the check compares against the live tip, not the
reservation's own window.

Reproduced live on F-0069's adjudicate session: claimed `S:53204-53253` at
`2026-10-06T205646Z`, 10 Stories derived and ready to mint; by `2026-10-06T212234Z` the record's
highest `S-` id was already `S-53507` (53 Story cards minted by other jobs in between), so every
`asf new story` call failed with `new: id range S:53204-53253 exhausted` — a block that was never
used by its own holder, reported exhausted anyway.

## Why it matters

This is a plausible root cause for at least part of the `NO STORIES -> SPEC-AMEND` stalemate
pattern this backlog keeps hitting (F-0069, and the same message on F-0113, F-0177, F-0179): a
spec-amend/adjudicate session is handed a block, cannot use it the instant it is claimed (it has
to read the spec, the plan and the landed Tasks first), and by the time it calls
`asf new story` the block is dead on arrival. The session then has no lower-risk way to retry:
the only fallback inside `mint_id` for a *matched* range is the `SystemExit`; nothing falls
through to the single-id claim-by-push path while `BACKLOG_ID_RANGE` still names the type's
prefix.

## What a fix might look like

Not diagnosed here (out of this finding's scope): either claim-verify the block's ref is still
unconsumed and extend/reissue it past the live tip on exhaustion instead of refusing outright, or
let a matched-but-exhausted range fall through to `idclaim.claim_one`'s single-id path the way an
unmatched range already does.
