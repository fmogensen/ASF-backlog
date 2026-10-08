# an item resolved by one landing can't take a follow-up fix: the lane closes the PR and asf reopen is refused

Severity: S2

On 2026-10-08 the factory lane closed fix PR #1244 (fix/B-82960-parked-holder-row, green) with "B-82960 is Resolved in the record". The PR was a follow-up fix for the same item, after #1239 had landed an incomplete fix. `asf reopen B-82960 --reason …` was then refused: "current evidence still says Resolved (rule: fixed): merge 14b963b … lands B-82960". An item cannot get a second fix once one landing marks it fixed, and a person's reopen can't override the evidence.

## Acceptance
- `asf reopen <id> --reason …` reopens the item even when a landing's evidence says fixed. The reopen is recorded with its reason, and a later landing re-resolves it. A test covers it.
- The lane does not close an open, green fix PR for an item that a person reopened. A test covers it.
- A fix PR for an already-Resolved item that the lane does close says so in one line and names `asf reopen` as the way to land it.
