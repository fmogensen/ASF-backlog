→ B-223558

# Satisfied on main closes items whose work was never built: 9 false closes
parent: F-0360

Type: Bug

"Satisfied on main" closes items whose work was never built. Measured 2026-10-14 06:30: 9 Bugs filed on 10-13/14 read kernel_state done with the note "satisfied on main at <sha>: <test ids>", and no merged PR names any of them (B-211240, B-211241, B-213994, B-214946, B-216547, B-216548, B-218650, B-218951, B-222657). One of them (B-214946) had its PR #1738 closed unmerged. Their acceptance lines cite existing test modules or classes (tests.kernel.test_rereview_scope, tests.kernel.test_reviewer_fixes.Bounds) as the place the new tests go; those ids already pass on main, so the rule reads the work as done. The auto-filed "Later findings" Bugs cite the same modules and close within a tick of being filed. Security hardening (a forged reviewer-fix trailer) and the revert-PR auto-merge follow-ups are among the lost items.

## Acceptance
- [ ] An item is satisfied on main only by test ids that did not exist on main when the item was filed (absent at the trunk sha of its mint), each at class.method level; a module or class id that already existed never satisfies.
- [ ] On the first tick after install, every item done only by "satisfied on main" is re-judged by the new rule, and one that no longer holds goes back to New with a History line naming the old note.
- [ ] A test replays B-216547's card: its acceptance cites tests.kernel.test_rereview_scope, which exists at its mint sha, and it is not satisfied.
