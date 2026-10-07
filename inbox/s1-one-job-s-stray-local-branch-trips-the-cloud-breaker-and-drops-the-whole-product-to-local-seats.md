# S1: one job's stray local branch trips the cloud breaker and drops the whole product to local seats

Parent: E-0001
severity: S1

A cloud create is refused when the job's branch "exists locally with N commit(s) not on origin/main and no worktree — look before relaunching". Each refusal counts toward the cloud breaker, and 3 failed creates in 30 min switch the whole product to the local lane. A leftover local branch for one job therefore takes away cloud capacity for every job. Seen 2026-10-07 08:0x-08:2xZ on both products. Botseon tripped on cloud/spec-F-1146, and ASF on worker/T-0431 (earlier on spec/F-0026). Correct, review and coder jobs all fell back to the 4 local seats instead of 8 cloud ones.

## Acceptance
- A refusal caused by one job's own branch state (a stray local branch, unpushed commits, a missing worktree) does not count toward the cloud breaker. Only runtime or API failures (create errors, auth, quota, transport) count.
- A stray local branch with unpushed commits and no worktree is saved, not left blocking. Its commits are pushed to a recovery ref (`refs/asf/recovered/<branch>/<ts>`) on origin, the local branch is deleted, and the job launches on the next pass. Each save is logged, and the recovered ref is named in the item's History.
- The breaker's reason in `asf status` names the failure class. When stray-branch refusals are the only cause, the breaker never trips.
- Tests:
  - three stray-branch refusals leave the breaker closed;
  - three API failures trip it;
  - a stray branch is saved to a recovery ref and its job then launches.
