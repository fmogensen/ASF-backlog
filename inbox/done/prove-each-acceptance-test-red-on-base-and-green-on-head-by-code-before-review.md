→ retired: Superseded by S-203318 under F-0358 (milestone 0.4), which carries the same proof. This note was minted as S-185817 on 2026-10-12 but the card was never committed (it stayed an untracked file in the record checkout through a kernel outage), so nothing was built from it. (2026-10-13)

# Prove each acceptance test red on base and green on head, by code, before review
parent: F-0346

type: story
parent: F-0346
severity: S1

Prove each acceptance test red on the base and green on the head, by code, before review. Measured 2026-10-12 (last 12 h): 54 of 94 review verdicts were "changes" (57%). Of 112 findings, the largest classified group (31) is weak tests: tests that don't assert the line, build their expected value from the call under test, or pass whatever the state is. Next come wrong API or contract (16) and missed acceptance (8).

## Change
- A CI job (or kernel pre-review step) on every Task or Bug PR runs each test named in the item's Acceptance and `Proves:` claims twice: on the PR base tree (it must FAIL or ERROR, unless the line is marked pre-existing) and on the head (it must PASS). A test green on the base proves nothing: the PR is sent back by code with "test <id> passes without the change", before any LLM review.
- The build brief states the rule up front: write the acceptance test, watch it fail on the base, then make it pass (the fix-bug brief already says this; extend it to every coder brief).
- The review checklist row "test asserts the line" is pre-filled from this check.

## Acceptance
- A PR whose named test passes on the base is returned with that finding, and no review launches (test).
- A PR whose named tests fail on the base and pass on the head proceeds to review (test).
- Measured: the weak-test share of findings and the first-review approve rate, against 43% (2026-10-12).
