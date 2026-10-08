# repo .githooks fail in cloud sessions with no asf binary on PATH

Severity: S2

Reported by botseon on 2026-10-08 (T-0436 report cb08ca5e2). In the cloud sandbox there is no `asf` binary on PATH, so the repo's `.githooks` fail to start there, and commits or pushes are refused or skipped without a clear cause.

## Acceptance
- A hook started where no `asf` is installed exits 0 with one line naming the gap, or the cloud brief installs the pinned `asf` before work starts. Either way, a cloud session never fails on a missing hook binary. A test covers the hook with no asf on PATH.
