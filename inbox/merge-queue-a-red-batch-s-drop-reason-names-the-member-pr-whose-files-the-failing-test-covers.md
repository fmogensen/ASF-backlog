# Merge queue: a red batch's drop reason names the member PR whose files the failing test covers

When a merge-queue batch with several PRs goes red, the drop reason names only the failing check. Finding the PR that broke it then takes a hand diff grep. Seen 2026-10-06 on botseon batch 1ce016d (#1122, #1194, #1195): #1122 added audit writes that a known-gap test did not expect.

## Acceptance
- A red batch's drop reason lists the failing tests and, for each member PR, whether its changed files overlap those tests or the files they cover. The overlap comes from the test file's path, its imports, and the files it names. The suspect PRs are ranked first.
- When exactly one member overlaps, the queue names it as the likely culprit in the drop reason and in the ledger.
- The overlap is computed in code with no LLM call. It is generic over products, using the product's test paths from config.
- Tests: a 3-PR batch with one overlapping PR names that PR; no overlap says "no member overlaps" and lists all members.

## Question
This reads as a defect. A Bug carries a signature — add signature: <the failing test or error line>, or paste that line into the body (an `Error:` line or a `file:line › test` line is read as one); or an ## Acceptance list if it is new work.
