# Bug: a refused push strands a local session's work on a local-only branch
parent: F-0346

type: bug
parent: F-0346

Bug: a local session whose push is refused leaves its work stranded on a local-only branch, silently. On 2026-10-10/11 the product pin pointed at a CLI with no `pre-push`, so the git hook refused every local push. 17 worker branches held unpushed commits (T-129178 among them), the kernel marked 10 of those items falsely satisfied, and the console recovered them by hand by pushing the branches.

## Acceptance
- When a local session ends, the kernel compares the session's local branch with origin. Commits that aren't on origin get one push by the kernel, and if that fails the item becomes Stuck with the reason "local commits not on origin: <n>, push refused: <error>" (test).
- A refused push in a session report is a finding, never a done verdict (test).
- `asf kernel status` counts local worker branches with commits that aren't on origin and aren't held by a live session, as a flow violation (test).
