# Enforce the factory-only merge rule in ASF's own CI (the freeze is config-only today)

The hand-build freeze is on (2026-10-06): ASF's own product config sets `conventions.merge: {mode: auto, factory_only: true}`. But ASF's CI does not run the check yet (`python3 -m asf.factory_only`, added in round E, #780), so a hand PR into main still passes CI — the rule exists only in config the CI cannot read (operator config lives outside the repo).

## Acceptance
- [ ] `.github/workflows/tests.yml` (or its own workflow) runs `python3 -m asf.factory_only` on every pull_request into main, as a required check.
- [ ] The rule's inputs come from a committed, generic file in the repo (e.g. `.asf/product.yaml` with `conventions.merge.factory_only: true` and the factory `branch_prefixes`), not from operator config; the factory's own branches pass, a `fix/…` or `feat/…` hand branch is refused, release tags and CHANGELOG-only PRs pass.
- [ ] Tested: the workflow step's refusal and pass cases (hermetic, using the module's --head/--base/--files args).
- [ ] Docs: docs/guide/operating.md explains how a product turns the rule on.
