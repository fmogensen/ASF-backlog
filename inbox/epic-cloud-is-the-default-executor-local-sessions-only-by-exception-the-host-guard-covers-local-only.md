# Epic: cloud is the default executor — local sessions only by exception; the host guard covers local only

type: epic

## Why
On 2026-09-26 most waves were held by "host pressure load 30–45 (1m up to 112)/cores 10", while CI runners sat partly idle. botseon ran 0–6 sessions for hours. Every worker session, and its typechecks and vitest, shares one 10-core Mac with the controller, ASF's own sessions and Spotlight. The operator: "why do we run things on the Mac … when we have cloud agents and cloud CIs".

## Today
The cloud lane exists: asf/workers/cloud.py, runtime `actions`. It runs `claude -p` in the product's `asf-worker` workflow on the product's own runners, and pushes the branch as a local session does. Harvest is unchanged. It is **overflow only** (no local seat, or host pressure), and it is **off**. The runtime `claude-cloud` is refused, because the CLI cannot create a cloud session non-interactively.

## Target
- Cloud is the default executor for coder, correct, review, adjudicate, fix-bug, spec, plan, direct and groom.
- Local runs only by exception: a row kind or item flagged `local-only`, such as Docker, computer-plane, local credentials or devices.
- The host guard applies to local sessions only. Cloud sessions are bounded by `cloud.max_inflight`, the runner pool and account quota.
- Stays local: the controller (tick, harvest, record, local gate), and sessions that need the host.

## Blockers
1. **Credential.** The repo secret `CLAUDE_CODE_OAUTH_TOKEN` (one per product repo) is operator-only, so ASF can't create it. The same goes for each cloud account's token.
2. **Workflow.** The product repo must commit `.github/workflows/asf-worker.yml` (`asf cloud install` writes it).
3. **Runner capacity.** A worker job holds a CI runner for up to 4 h, competing with CI. It needs its own runner class (`class-agent`) or GitHub-hosted runners, so the CI queue doesn't starve.
4. **Quota.** Cloud sessions draw on the same account 5h/7d windows. The pool's quota guard must count them (it does, through the role: cloud accounts), and the accounts must be granted access to the repo.
5. **Worktree and branch.** The branch is created on origin before dispatch, and the local worktree is synced after the run (cloud.py does this). Needed: conflict and rebase handling when the factory rebases a cloud branch mid-run.
6. **Harvest from cloud pushes.** It works through the report-commit end marker. Needed: an end-to-end proof on a real product run.
7. **Evaluate** creating claude.ai cloud sessions through the remote-trigger API, as a second runtime once it can be driven non-interactively.

## Slices
1. **Placement:** `cloud.default: true` puts eligible rows in the cloud first, and `local_only` kinds/flags stay local. The host guard is scoped to local rows. `asf cloud doctor` reports readiness (secret present, workflow on trunk, runner label online, accounts) with one line per gap. Tests.
2. **Runner class:** `cloud.runs_on: [self-hosted, class-agent]`, counted by ci_queue as its own class and never taking CI slots.
3. **First live run:** one botseon review row in the cloud, end to end (dispatch → push → harvest → merge), then widen to coder, correct and fix-bug.
4. **Default on:** set it for botseon, measure sessions in flight and lead time against the local-only week.

## Question
Which Epic is this under? No open Epic shares a title word with it.
