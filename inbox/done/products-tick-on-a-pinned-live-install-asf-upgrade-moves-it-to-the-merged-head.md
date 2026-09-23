→ F-0104

# Products tick on a pinned live install; `asf upgrade` moves it to the merged head
type: feature
parent: E-0002
severity: S2

Today every product's launchd job runs `sys.executable -m asf.cli tick` from the editable dev install (`pipx install --editable ~/Code/fmogensen/ASF`, venv asf-factory). A product tick therefore runs whatever sits in the dev checkout, uncommitted edits and other branches included, the moment it changes. D-0045 said products install `git+…/ASF@vX` and upgrade via `asf upgrade`; that never happened, no tags exist, and `asf upgrade` runs `pipx upgrade asf-factory` (the editable dev venv). pipx upgrade on a git URL with an unchanged version (0.1.0) is also a no-op.

Operator stopgap 2026-09-23: botseon ticks from a second install, `pipx install --suffix=-live git+https://github.com/fmogensen/ASF.git@<sha>` (venv asf-factory-live). It is pinned to a sha and upgraded by hand with `--force`.

Expected:
- `asf upgrade` upgrades the live install (the one the scheduler's plists name), not the dev venv: it reinstalls it at origin/main's head (or the newest tag once releases exist), then runs the schema check for every product. It refuses while sessions are in flight, and it records the installed sha.
- `asf doctor` shows a row: which install each product clock runs, its sha, how far behind origin/main it is, and RED if a product clock points at an editable install.
- A scheduled step (daily, or after harvest lands on main) runs the upgrade, so merged ASF work reaches products without a hand step, and unmerged work never does.
- Rollback = reinstall the previous recorded sha; `asf upgrade --to <sha>`.

History:
- 2026-09-23 operator: "how does botseon get updates when ASF updates as it does continuously?" — then accepted the recommendation: a clean live install that only takes merged work, with a rollback.
