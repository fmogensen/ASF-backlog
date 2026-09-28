→ B-0150

# doctor ci-pool row flags ASF's own ci_pool.reserve label as undeclared
signature: ci pool RED: provider-like label in runs-on: ci.yml asks for 'class-pr-heavy', not a declared role (heavy, light)
severity: S3
parent: E-0002

`asf doctor --product botseon` reports the `ci pool` row RED: "provider-like label in runs-on: ci.yml asks for 'class-pr-heavy', not a declared role (heavy, light)".

The label is ASF's own. botseon.yaml declares `ci_pool.reserve: {label: class-pr-heavy, of: heavy, keep_free: 2}`, the CI queue applies that label to the reserved runners on every tick, and the product's ci.yml asks for it because the reserve exists.

Wanted: the doctor's pool check accepts a label declared under `ci_pool.reserve` as a derived role of its `of:` role. Add a test where a reserve label is used in runs-on and the row comes out ok.
