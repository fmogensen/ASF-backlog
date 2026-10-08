# Finished reviews are dropped when the review worktree is not a checkout, so green PRs get no approval and the row parks

Bug. Green, mergeable, non-parked factory PRs (#1191 T-78560, #1184 T-76352, #1142 T-0709 and many earlier ones) never land: the review session ends `status done` with a clean verdict, but the health pass logs `review not filed: .../worktrees/review-t-NNNN is not a git checkout: the review of T-NNNN on worker/T-NNNN cannot be filed from it` (asf/evidence/review_store.py NotACheckout). No review entry exists, so the lane says "PR not approved - no ASF review yet", and the row is then PARKED ("review-t-78560 launched 1 time(s) ... last report: status done. Not relaunched") until a new commit or `asf unpark`. The review's work is thrown away and nothing retries. Seen 2-4 times for each of review-t-0817, t-0807, t-0709, t-0812, t-0799, t-0720, t-0663, t-0591 ... in tick-asf-*.log. The review session's sandbox refuses writes under .git/worktrees/review-<id>, so the worktree is not a checkout at harvest.

## Acceptance
- [ ] A finished review whose worktree is not a git checkout at harvest is recovered, not dropped: the verdict in the session's REPORT is filed against the PR head, or the review is relaunched once on a fresh worktree, and the PR leaves "no ASF review yet" within one wave
- [ ] A review that ended `status done` with a REPORT but no filed entry is never counted as a launched review by the park rule (the row is not PARKED on "launched 1 time(s)")
- [ ] The wave prints one waits line naming the unfiled review and the action taken, never a silent park
- [ ] A test builds a review worktree whose gitdir is missing, runs the harvest, and asserts a review entry exists or a relaunch is queued, and the row is not parked

## Question
Which Epic is this under? No open Epic shares a title word with it.
