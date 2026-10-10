# Bug: plan_on_approve never mints an approved spec's declared Stories

Bug (defect): plan_on_approve launches a plan on an approved spec but its declared Stories are never minted

Type: bug. A fix exists on an open PR (fix branch for F-0350, which is a framework fix, not F-0350's delivery); this card owns that fix so the kernel never mistakes it for F-0350's own PR.

## Defect

With `plan_on_approve` a plan launches on an approved, unmerged spec, but the Stories the spec declares were only minted once the spec landed. The plan session reported "the six Stories are declared in the spec but the record holds no card for any of them ... no plan session can start".

## Fix

- facts: `Facts.specs_approved` holds the spec text at the head of every open spec PR whose last verdict on that head is `approve`, read by `RealRecord.spec_at(pr)` (`git show <head>:<specs_dir>/f-<n>.md` in the product checkout; the branch fetched once when the commit is missing, never under the mutation guard).
- decide: with `plan_on_approve`, `_mint` mints the declared Stories of an approved spec head (approval re-checked on the head) as it does for a landed spec, idempotently: an id on the record, or already minted this tick, is never minted again.

## Acceptance (tests)

- tests/kernel/test_stories_on_approve.py: decide mints an approved spec head's Stories once and never twice; facts reads `specs_approved` only for heads whose last verdict is approve; the real reader works over a temp git origin.
