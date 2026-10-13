→ B-213995

# A revert PR held for want of a guilty blame read is never closed
parent: F-0357

Type: Bug

A revert PR held for want of a guilty blame read is never closed (follow-up to PR #1725). asf/kernel/mainline.py `judge` returns "the revert stays held" when the revert's CI is green and `blame.verdict` is not guilty; nothing closes that PR when the verdict settles trunk-wide or unproven, or when the red spell ends.

## Acceptance
- [ ] A held revert whose blame settles trunk-wide is closed with a note naming the verdict.
- [ ] A held revert whose blame settles unproven (not asked) is closed once the spell's Bug is filed.
- [ ] A held revert whose blame is still asked stays open.
