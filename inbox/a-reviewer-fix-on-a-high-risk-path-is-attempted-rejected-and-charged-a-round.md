# A reviewer fix on a high-risk path is attempted, rejected and charged a round
parent: F-0358

Type: Bug

A reviewer fix on a high-risk path is attempted, rejected and charged. Under kernel.review.fix: on (PR #1742) the kernel rejects a fix that touches a kernel.risk.high path, records the verdict as changes and charges a normal round. For this product kernel.risk.high covers the kernel's own directory, which most items write, so a fixing review that touches it wastes its work and pushes the item toward the fix-round cap: T-210362 went Stuck on the operator after its third round with "reviewer fix out of bounds (a high-risk path: asf/kernel/decide.py)".

## Acceptance
- [ ] The fixing review's brief lists the paths a fix may touch (the item's writes and test paths minus kernel.risk.high) and says to report findings on any other path instead of fixing them.
- [ ] An item whose writes are all high-risk gets a review without fix authority.
- [ ] An out-of-bounds reviewer fix charges no round to the item; the next round carries the review's findings.
