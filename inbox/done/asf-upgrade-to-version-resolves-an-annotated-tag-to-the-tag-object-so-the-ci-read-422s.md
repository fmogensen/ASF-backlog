→ F-0303

# asf upgrade --to <version> resolves an annotated tag to the tag object, so the CI read 422s

Parent: E-0001
severity: S2

`asf upgrade --to <version>` resolves an annotated tag to the tag object, not the commit it points at. `resolve_target` maps v0.1.254 to tag object 78242c1 instead of commit 5c94c907, the GitHub check-runs read returns 422, and the upgrade stops with "CI unknown (gh could not read the check runs)". Passing the commit sha works. Seen 2026-10-08 on botseon (0.1.245 → 0.1.254).

## Acceptance
- resolve_target peels a tag to its commit (`<tag>^{commit}`) before every host read and every pin. An annotated and a lightweight tag both resolve to the commit. Tested.
