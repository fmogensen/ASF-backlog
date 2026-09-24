# Onboarding: adopt a product's in-flight PRs (asf adopt-pr + legacy review form) so native landing drains them

Every product that migrates onto ASF has a queue in flight: open PRs its previous process opened, with its own review convention. ASF's native PR landing only merges PRs ASF opened, with an ASF review file (`verdict: approved`). So the product has to keep its old merge machinery running until that queue drains, or strand the PRs. Reported by the first customer (about 20 open pre-ASF PRs).

Want:
1. `asf adopt-pr <n>… [--item <id>] --product P` (and `--all` with a branch glob): registers each open PR as an ASF lane run.
   - The item is derived from the branch or title via the product's branch_patterns, or given with --item.
   - It writes a run line in the session ledger, marked as adopted, never launched, so harvest's native landing and health treat it like any ASF PR.
2. `conventions.legacy_review`: {glob, verdict_regex}, e.g. glob `<reviews_dir>/<slug>-r<n>.md` with regex `\(APPROVED\)`. It's accepted as the review for adopted PRs only, alongside ASF's own format.
3. The adopted queue drains through native landing with the same checks, approvals, ordering and capacity. `asf status` shows "adopted: N open" until it's empty, and doctor then says the product's old batch step can be retired.
4. The guide's onboarding page documents it: adopt the in-flight queue, then turn the old merge step off.

Tests with a fake gh:
- adopt → a ledger line; an adopted PR with a legacy APPROVED review and green checks → merged
- no review → waits
- `--all` with a glob → only matching PRs
- adopting the same PR twice is idempotent
