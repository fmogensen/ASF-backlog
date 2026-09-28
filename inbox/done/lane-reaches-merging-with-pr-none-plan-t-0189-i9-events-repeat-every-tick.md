→ F-0229

# Lane reaches MERGING with PR #None (plan/T-0189); I9 events repeat every tick
parent: E-0001

The lane tried to merge plan/T-0189 with no PR number and logged `held plan/T-0189: PR #None merge refused — no pull requests found for branch "None"`, followed by `lane: plan/T-0189 MERGING → WAITING`.

The branch reached MERGING without a PR number, and the merge call passed the string "None" as the branch name.

Wanted:
- MERGING requires a known PR number. If there is none, look the PR up by the branch's head, or send the branch back to PR_OPEN.
- Add a test covering it.

Related noise: every tick repeats `EVENT I9: fix/... merged outside the lane` for the same three branches (fix/merge-refused-conflict-back, fix/merge-refused-conflict-mergeable, fix/quota-stale). An I9 event should be recorded once per branch.
