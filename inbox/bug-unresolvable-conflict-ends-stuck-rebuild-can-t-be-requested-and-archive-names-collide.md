# Bug: unresolvable conflict ends Stuck, rebuild can't be requested, and archive names collide
parent: F-0346

type: bug
parent: F-0346

Bug: an unresolvable conflict ends Stuck(operator), and an operator answer can't ask for a fresh rebuild. Measured 2026-10-12: T-159852 (PR #1624) went Stuck twice on "conflict the rebase session could not resolve". The kernel does archive-and-rebuild by itself for this case (T-173236 at about 01:00 local: "rebuild T-173236: archive worker/…, close #1617"), but not here. An answer saying "archive and rebuild fresh" was applied as "one more fix round on the strong model", which rebased again and conflicted again. The console then archived by hand and found `archive/worker/T-159852` already existed, so the archive push was refused (non-fast-forward). The kernel's own archive step would hit the same collision.

## Acceptance
- A conflict the rebase session can't resolve leads to archive-and-rebuild (bounded by the round cap) and never to Stuck(operator) while rounds remain (test).
- An operator answer starting with `rebuild` (or `asf rebuild <id> --why`) archives the branch, closes the PR and relaunches from origin/main (test).
- Archive names never collide: an existing `archive/<branch>` gets a numeric suffix, and the old archive is never overwritten (test).
