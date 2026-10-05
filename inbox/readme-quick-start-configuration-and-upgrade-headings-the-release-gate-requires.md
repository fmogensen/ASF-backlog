# README: Quick start, Configuration and Upgrade headings the release gate requires
parent: F-0030

parent: F-0030

`asf release-readiness --product asf` criterion 7 (docs) fails on: "README lacks Quick start, Configuration, Upgrade". `asf/release.py` DEFAULTS `readme_sections: [Install, Quick start, Configuration, Upgrade]` matches each as a substring of a README heading. The README today has only `## Install`.

F-0030's open Task T-0138 cuts the page into `## The argument`, `## The mental model`, `## The manual`. Its manual carries the install block, the quickstart block and "~/.ASF as the operator's configuration" as prose, but no heading with any of the three missing words, and nothing in T-0138, T-0139 or F-0108's T-0413..T-0416/T-0421 writes an Upgrade section. So F-0030 can land and the criterion still fails.

## Acceptance
- [ ] The README carries headings containing "Quick start", "Configuration" and "Upgrade" (e.g. `### Quick start`, `### Configuration`, `### Upgrade` under `## The manual`). The quickstart stays the block T-0139 executes.
- [ ] Configuration: where ~/.ASF/config.yaml and products/<p>.yaml live, the minimum keys, and a link to docs/guide/product-config.md. It names no operator and no product, so check_generic passes.
- [ ] Upgrade: `asf upgrade`, the product's `upgrade: auto|notify|off` key (F-0112) and what a schema migration does on upgrade (F-0114), with links rather than restated mechanics.
- [ ] A test asserts `release.readme_headings(README)` covers every `readme_sections` entry, so the docs criterion's README half is proved in CI.
