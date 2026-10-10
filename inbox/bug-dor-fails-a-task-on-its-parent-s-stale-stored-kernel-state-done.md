# Bug: DoR fails a Task on its parent's stale stored kernel_state done

Bug (defect): DoR fails a Task on its parent's stale stored kernel_state "done"

Type: bug. A fix exists on an open PR (fix branch for F-0318, which is a framework fix, not F-0318's delivery); this card owns that fix so the kernel never mistakes it for F-0318's own PR.

## Defect

Features still store `kernel_state: done` from the old "plan merged -> Feature done" defect (fixed in #1404). `dor.missing` read the parent's card state literally, so their child Tasks failed DoR ("parent ... is Done") and groom-fill sessions were wasted re-checking it.

## Fix

- decide.hold_unready passes `parent_done` to `dor.missing`, computed with the tick's own `_state_of` (a container with children takes its state from them, never from a stored value), or the record itself closed it.
- model/ports: `Item.closed` is the record's own `state: Closed/Resolved`, kept apart from a stored `kernel_state`.

## Acceptance (tests)

- tests/kernel/test_dor.py::StaleParentDone: a child Task under a Feature whose stored kernel_state is done but whose children are open passes the parent check; a parent the record closed still fails it.
