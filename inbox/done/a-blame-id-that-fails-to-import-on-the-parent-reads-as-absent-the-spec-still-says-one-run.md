→ B-213994

# A blame id that fails to import on the parent reads as absent; the spec still says one run
parent: F-0357

Type: Bug

asf/kernel/trunk.py `missing_ids` reads any loader stand-in as "absent on the parent", including a module that exists there but fails to import on the host (follow-up to PR #1725). The run on the red commit now guards the verdict, but the result and the revert body still say the ids "do not exist". Also docs/specs/f-0357.md D1 and section 2.2 still describe one run on the parent; the code runs the ids on the parent and on the red commit, and retries an unanswerable read at most twice.

## Acceptance
- [ ] An id whose module file exists in the parent tree and fails to import is an error, not `missing`.
- [ ] An id whose module, class or method is absent in the parent tree is `missing`.
- [ ] The spec describes the two-run read and the bounded retry.
