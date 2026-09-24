# asf install: one command from zero to a ticking factory (from a release tag, doctor green)
parent: E-0001

parent: E-0001

Operator, 2026-09-24: "Do we have asf installer ready?" No. The pieces exist, but nothing takes a machine from zero to a ticking factory. Today an install takes all of these steps by hand: `pipx install -e <checkout>`, hand-written `~/.ASF/config.yaml` and `products/<p>.yaml` copied from docs/*.example.yaml, `asf init`, `asf scheduler install`, `asf hooks`, `asf plugin build` plus a manual plugin install in Claude Code, and a hand-made worker account pool. The README names `sample/` as the quickstart, but no install steps are written down anywhere. v0.1.0 is tagged, and an installer is what makes it usable by anyone else.

## Acceptance
- [ ] `asf install` (or a one-line bootstrap script) installs the package from a release tag, not a checkout. It asks for, or takes flags for, the product repo, the record repo and the scheduler kind, then writes `config.yaml` and `products/<p>.yaml` from the documented schema. No hand editing.
- [ ] It runs `asf init` (the record), `asf hooks` (redaction), `asf scheduler install` (the clocks) and the plugin install into Claude Code, and it detects or sets up at least one worker account.
- [ ] It ends with `asf doctor`, all green, and one `asf tick` dry run. It is idempotent: rerunning it repairs rather than duplicates.
- [ ] `asf uninstall` removes the jobs and hooks and leaves the record and repos untouched.
- [ ] A README "Install" section of 5 lines or fewer, and a CI test that installs into a temp HOME against `sample/` and gets doctor green.
- [ ] Relates to F-0104 (ticking on a pinned install; `asf upgrade` moves it).
