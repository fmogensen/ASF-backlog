→ F-0357

# A red trunk is handled correctly: blame is established before any revert
parent: E-0007

Type: Feature
Priority: need

## Description
A red trunk is handled correctly. On 2026-10-13 one clock-dependent test turned the trunk red. The kernel blamed the newest merge, opened a revert of an innocent PR, charged fix rounds to open PRs for a failure none of them caused, archived a Task for it, recorded the unmerged revert as done and then rebuilt that Task in a loop. The console paused the kernel, fixed the test and broke the loop by hand. Groups B-200581 and B-200932.

## Acceptance
- [ ] Before reverting, the kernel establishes blame: the failing test must pass on the parent of the suspect merge (or fail on the revert PR means not guilty). A failure that is red without the merge is classed trunk-wide.
- [ ] Trunk-wide red: launches of new builds are held, no revert is opened, no open PR is charged a fix round or archived for that test, and exactly one Bug is filed naming the test and the first red commit, at the front of the queue.
- [ ] A revert counts only when the revert PR has merged. An item satisfied on the trunk is never relaunched; a build that ends with an empty diff marks the item Done.
- [ ] No test or product code reads the wall clock directly: one clock helper, a lint that refuses others, and the three known reads (ci_stall, ci_cancels, doctor) take an injected now. The suite passes with the clock moved 30 and 400 days forward in CI.
- [ ] Replay test of the 2026-10-13 incident: no RevertPR, no fix round, no archive, one Bug.
