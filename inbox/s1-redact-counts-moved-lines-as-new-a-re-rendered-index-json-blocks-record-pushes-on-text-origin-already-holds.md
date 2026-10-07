# S1: redact counts moved lines as new — a re-rendered index.json blocks record pushes on text origin already holds

Parent: E-0001
severity: S1

The pre-push redaction scan treats every line in a commit's `-U0` diff as new content (asf/redact.py `scan_unpublished`, around lines 359-381: `git show -U0` then `_parse_diff_added_lines`). A regenerated file whose lines only moved therefore reports every moved line as a new finding, even though origin already holds that exact text. index.json is the usual case: `asf set` re-renders it with lines reordered. The record push is then refused and the record commits stay local. Seen 2026-10-07 07:58 on botseon: 6 `asf set` commits were blocked by 78 findings ("name (worker_pool.accounts)"), all of them existing card titles already on origin. They stayed blocked after a rebase.

## Acceptance
- For each file in an unpublished commit, a line counts as added only if its exact text is absent from the parent's version of that file, or from the published remote tip's version. Moved or reordered lines produce no finding. Tested with a reordered index.json carrying a pattern-matching title that origin already has: no finding.
- A genuinely new matching line in the same commit is still found. Tested.
- index.json is rendered in a stable order (by id), so a one-card `asf set` changes only that card's lines and its reverse links. Tested: setting one field changes at most the lines of the cards it touches.
- A record push refused by redact is reported as a named finding in `asf status` and `asf doctor`, not only in the log.
