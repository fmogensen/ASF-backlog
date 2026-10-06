# asf statusline: each session shows its own product's status (generic, from tick facts)

Operator 2026-10-06: "each product should show its own status right?" Today the operator's status line is a hand-kept host script that hardcoded one product's counts for days (ASF's own cloud sessions were invisible). It should be a generic ASF command.

## Acceptance
- [ ] `asf statusline [--cwd <dir>] [--json]` resolves the product whose `repo_dir`/`backlog_dir` (or a worktree under the product's state dir) contains the cwd, and prints one line: `<product> <vX.Y.Z> · local L/S · cloud C/M · reds24h R · next N` from the product's own pinned install facts (no model calls, < 1 s from cached tick facts).
- [ ] Outside any product it prints one compact segment per configured product.
- [ ] Reads only facts the tick already writes (no gh/API calls on the render path); stale facts are marked with their age, never shown as fresh.
- [ ] `asf install` offers to wire it as the Claude Code statusLine command; documented in docs/guide.
- [ ] Hermetic tests: cwd → product mapping (repo, backlog, worktree, none), stale marking, two-product fixture.
