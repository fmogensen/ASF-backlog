→ B-218951

# A reviewer fix is attributed by a trailer any commit author can write
parent: F-0358

Type: Bug

A reviewer fix is attributed by the `ASF-Review-Fix: <job>` trailer alone (asf/kernel/reviewfix.py `check_bounds`), which any commit author can write. The hook-stamped `ASF-Session` trailer (asf/workers/githooks.py) is the real identity, but the session id is not on the kernel's `Session` facts. The independent verify review still guards landing, so this is hardening.

## Acceptance
- [ ] The review run's session id reaches `Session` and `check_bounds`.
- [ ] A commit whose `ASF-Session` is not the fixing review's is out of bounds.
- [ ] A test for a forged `ASF-Review-Fix` trailer from another session.
