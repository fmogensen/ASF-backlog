→ B-195724

# asf redact --pre-commit refuses a conflicted trunk merge: it scans the trunk's own files
parent: F-0346

Type: Bug

`asf redact --pre-commit` refuses a conflicted merge commit of the trunk into a branch. A conflicted merge stages the trunk's own files, and the hook scans them as if the branch had written them. Clean merges pass only because git skips pre-commit for them. Reported by the botseon console on 2026-10-12: 11 worker-account-name findings, all in files byte-identical to the trunk, so a PR that conflicts with the trunk could not be refreshed by merge and had to be rebuilt.

A second, related problem: a product trunk can itself already carry redaction findings, so `asf redact --tree` on the trunk is red.

Acceptance:
- During a merge, the pre-commit redaction scan covers only content that differs from both parents (the resolution); paths identical to MERGE_HEAD are skipped.
- Test: a conflicted merge where the trunk side holds a finding and the resolution adds none commits clean; a resolution that adds a finding is still refused.
- `asf redact --tree` findings that already exist on the trunk are reported as trunk debt, with a count, and are told apart from findings a branch adds.
