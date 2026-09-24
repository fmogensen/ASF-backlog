→ B-0096

# A code branch re-gated as 'red outside its writes' can loop forever without being corrected
parent: E-0002

signature: re-gated next tick (red outside its writes), with no end
parent: E-0002

Hotfix 5df3afe (harvest: docs-only branches are never held for a red test) widened the "red outside its writes" rule: when an item has no `writes:`, it now uses the branch's actual diff. Known risk, named by the hotfix: a code branch with no `writes:` whose failing output names only a test file outside its diff is re-gated every tick and never sent back for correction. If the branch really broke that test indirectly (for example through an import), it loops forever without a session ever seeing the failure.

## Acceptance
- [ ] A branch re-gated as "foreign" N ticks in a row (default 3), while the same modules are green on trunk alone, is held back to its session with the failing output. The count lives in harvest state.
- [ ] `red on trunk too` resets the count, since the red is not the branch's.
- [ ] Tests for both.
