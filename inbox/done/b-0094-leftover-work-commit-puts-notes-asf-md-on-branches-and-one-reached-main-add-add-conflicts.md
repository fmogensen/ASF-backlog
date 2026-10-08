→ F-0321

# B-0094 leftover-work commit puts NOTES.asf.md on branches and one reached main (add/add conflicts)

Severity: S2

Reported by botseon on 2026-10-08. The B-0094 leftover-work commit writes `NOTES.asf.md` onto branches. One copy reached main (ef2566bf4) and causes add/add conflicts on later branches (T-0537).

## Acceptance
- The leftover-work note is never committed to a product branch: it goes to the record or to the session ledger, or `NOTES.asf.md` is in the landing's refused-path list. A test covers it.
- The landing refuses a PR that adds `NOTES.asf.md` to the trunk, by name.
