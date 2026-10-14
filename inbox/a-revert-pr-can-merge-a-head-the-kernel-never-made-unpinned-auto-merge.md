# A revert PR can merge a head the kernel never made (unpinned auto-merge)
parent: F-0357

Type: Bug

A revert PR can merge a head the kernel never made. `_Applier.RevertPR` (asf/kernel/apply.py) finalizes with `enable_auto_merge(number)`: an unpinned GitHub auto-merge. When the revert is opened with auto-merge on while its CI runs (asf/kernel/mainline.py, `auto_merge=confirmed == 'green'`), a later push to the revert branch is not cancelled by GitHub. `withdraw_auto_merge` (asf/kernel/decide.py) only runs for item PRs in `_judge_pr`, never for `Facts.revert_prs`. B-218952 closed this for item PRs only.

## Acceptance
- [ ] A revert PR is merged by the kernel pinned to the head it read green; no GitHub auto-merge.
- [ ] A revert PR whose head is not the kernel's revert commit is never merged; any auto-merge on it is withdrawn.
- [ ] A test pushes a commit onto a revert branch after finalize and proves it does not land.
