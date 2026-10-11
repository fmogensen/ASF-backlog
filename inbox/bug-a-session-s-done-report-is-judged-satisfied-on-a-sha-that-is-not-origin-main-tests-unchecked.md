# Bug: a session's done REPORT is judged satisfied on a sha that is not origin/main, tests unchecked

type: bug
parent: F-0346

The kernel's "found done by its session" path (asf/kernel/needed.py take_done/_done_unchanged) judged 10 items satisfied "on main at 14b963ba1" (B-126941 B-136993 T-129173 T-129178 T-129185 T-130213 T-130966 T-130967 T-131721 T-83143). 14b963ba1 is the old 0.1.265 install pin, not origin/main: each worker REPORT said `pushed: no` because the pinned CLI refused the pre-push hook, and quoted the pin's sha in prose. The kernel took the first REPORT sha that origin/main holds (the pin is an ancestor) plus the session's own tests claim, and never checked that the named test ids exist on origin/main nor that the REPORT's own commits landed. T-129178's tests.test_scratch is not on origin/main; its commit adea4d1a sat on a local-only worker/T-129178. Its dependant T-129180 then launched on nothing.

Fix: the done-by-session judgement runs against origin/main freshly fetched at tick start: every named test id must be defined in that tree, every `commits:` sha must be on it, and the note names that trunk sha. `asf reopen` undoes a false kernel_state done on a New card after re-judging it against origin/main.

Fixes: tests.kernel.test_still_needed
