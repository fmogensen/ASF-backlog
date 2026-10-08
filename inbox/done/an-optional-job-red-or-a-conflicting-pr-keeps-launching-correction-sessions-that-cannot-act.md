→ B-83471

# an optional-job red or a CONFLICTING PR keeps launching correction sessions that cannot act
signature: correction session launched on a PR whose required checks are green (optional site job red) or which is CONFLICTING, and it reports commits: none
parent: E-0001
severity: S2


Reported by botseon on 2026-10-08. Correction sessions launch for states a session cannot act on:
- T-47334: only the optional `site` job (Lighthouse /news) was red, and every required check was green.
- T-0654: three correction reports said "commits: none" while its PR #1233 was CONFLICTING.

## Acceptance
- A PR whose required checks (`conventions.landing_checks`) are all green lands, whatever optional jobs say. An optional red draws no correction row. A test covers it.
- A CONFLICTING PR goes to the rebase/rebuild lane, not to a correction session. A test covers it.
- A correction that reports "commits: none" twice for the same cause parks with the cause named and is escalated (see the blocker-weight S1), instead of relaunching.
