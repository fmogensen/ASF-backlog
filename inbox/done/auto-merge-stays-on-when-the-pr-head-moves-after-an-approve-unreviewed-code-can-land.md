→ B-218952

# Auto-merge stays on when the PR head moves after an approve: unreviewed code can land
parent: F-0358

Type: Bug

Auto-merge enabled after an approve is never withdrawn when the PR head moves. asf/kernel/decide.py `_judge_pr` emits `EnableAutoMerge` and asf/kernel/ports.py `enable_auto_merge` runs an unpinned auto-merge; nothing disables it. A later push (a fix round after a red check, or a reviewer fix) can then merge on green checks with no verdict on that head. This lets unreviewed code land.

## Acceptance
- [ ] When a PR with auto-merge on has no holding approve at its current head, the kernel disables auto-merge on that tick.
- [ ] Auto-merge is enabled pinned to the approved head where the host supports it, so a moved head cannot merge.
- [ ] A test: approve at head A, push to head B, no merge before a verdict on B.
