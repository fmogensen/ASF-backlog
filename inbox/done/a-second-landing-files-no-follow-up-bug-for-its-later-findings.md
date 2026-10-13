→ B-216548

# A second landing files no follow-up Bug for its later findings
parent: F-0358

Type: Bug

A second landing files no follow-up Bug (follow-up to PR #1735). asf/kernel/decide.py `later_bugs` skips an item once `kernel_later_filed` is set, and `kernel_later_findings` is never cleared. An item reverted off a red main and landed again, or rebuilt by ArchiveAndReset, keeps its old lines; LATER findings raised by re-reviews of the second PR are merged in asf/kernel/apply.py `_keep_later` but never filed.

## Acceptance
- [ ] An item that lands again after a revert, holding LATER findings not in its filed Bug, files one more Bug listing only those (tests/kernel/test_rereview_scope.py).
- [ ] A landing with no new LATER finding files nothing.
- [ ] Applying twice files one Bug per landing.
