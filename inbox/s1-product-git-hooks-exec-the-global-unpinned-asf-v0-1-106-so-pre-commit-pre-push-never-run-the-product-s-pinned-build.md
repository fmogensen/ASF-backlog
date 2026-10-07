# S1: product git hooks exec the global unpinned asf (v0.1.106), so pre-commit/pre-push never run the product's pinned build

Parent: E-0001
severity: S1

The git hooks `asf hooks install` writes exec the global `$HOME/.local/bin/asf`, an unpinned pipx venv (asf-factory, still v0.1.106 on the factory host), not the product's pinned build. A product's pre-commit (the index.json render) and pre-push (redact) therefore run months-old code whatever version the product was moved to, and fixes like F-0273 never reach its pushes. Seen 2026-10-07 on botseon: the record and code repos' .githooks exec ~/.local/bin/asf (0.1.106+4), botseon runs 0.1.217, and record pushes were still refused for 78 findings after the redact fix landed.

## Acceptance
- `asf hooks install --product P` writes hooks that exec the product-pinned launcher (`$HOME/.local/bin/asf-<product>`, which reads the clock plist's interpreter), never the global `asf`. Tested on the generated hook text.
- `asf upgrade --product P` re-checks P's repo and record hooks, and rewrites any that exec a different build. Tested.
- `asf doctor` flags a hook whose `asf --version` differs from the product's pinned version. Tested.
