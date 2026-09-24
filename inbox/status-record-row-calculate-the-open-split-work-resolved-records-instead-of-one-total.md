# Status Record row: calculate the open split (work, Resolved, records) instead of one total

## Description
- The status Record row prints `<n> open · <n> Active · <n> blocked · <n> no rule`. `open` is every live item not Closed, so it includes Resolved items (done, waiting to close) and record-only types (decision, rule) that are never work.
- An operator reading `578 open` has to derive the real work count by hand. In one product: 578 open = 7 Resolved + 103 records (88 decisions, 15 rules) + 468 work items. A by-hand derivation is where wrong or rounded numbers come from.

## Expected
The row (or an `asf status --record` drill-down) calculates the split itself from index.json with the same filter, and the parts sum to the total:
- open work by type (task/story/feature/bug/epic)
- Resolved awaiting close
- records (decision/rule)
- no rule, split by its evidence reason
A test asserts that the parts sum to the headline.
