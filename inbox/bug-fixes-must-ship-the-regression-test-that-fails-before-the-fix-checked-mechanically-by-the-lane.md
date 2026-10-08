# Bug fixes must ship the regression test that fails before the fix (checked mechanically by the lane)

Parent: E-0001
priority: need

Stable-core plan, item 6. Every fix for a product-reported defect lands with the regression test that would have caught it.

## Acceptance
- The review brief and rubric carry a rule: a Bug fix PR must add or change a test that fails on the parent commit and passes on the head. The lane checks it mechanically: it runs the changed tests on the parent and requires at least one to fail. Tested on a fixture repo.
- A Bug fix without such a test is sent back with that finding. Tested.

## Question
This reads as a defect. A Bug carries a signature — add signature: <the failing test or error line>, or paste that line into the body (an `Error:` line or a `file:line › test` line is read as one); or an ## Acceptance list if it is new work.
