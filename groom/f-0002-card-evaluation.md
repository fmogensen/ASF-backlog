# Card evaluation — F-0002

base: 8592f0ce2125de0dd14ce2966dd3b65f3f7c132d
evaluated: 42 items (selector: "migrated from the first product's backlog")
rubric: docs/specs/f-0002.md §2.2 in the ASF repository

## Notes

- F-0034 Harvest by code: the tick lands finished worker branches an… — reworded — `product:conventions.test_command`, `product:main`, `product:repo_dir` — "the main branch", "a product repository" and the test run now read the product's own main, repo_dir and test_command
- F-0035 Thin controller: every read loop moves to the tick, and a r… — reworded — `config:worker_pool.accounts`, `config:quota_guards.controller_warn` (new key), `config:scheduler.provider` — the controlling account is one of the shared pool, the warn band is a key, the scheduler is the configured provider
- F-0036 PR hygiene by code: approved and conflicting goes to a reba… — reworded — `product:repo_slug`, `product:stage_limits.pr_stale_dirty` (new key) — the pull requests are those of the product's repo_slug; the seven-day limit is per product
- F-0037 Questions in batches: a non-blocking question goes to the g… — clean — reads only the record's groom file and the tick's reply; both are per product
- F-0038 A factory-only CI class: a push touching only factory code… — reworded — `product:ci.factory_only_paths` (new key), `product:ci.workflow` — "the CI workflow" is the product's; the path list gets its key; "the product repository" made the product's own
- F-0039 Same-session correction: review findings go back to the wri… — reworded — `product:stage_limits.writer_dead` (new key) — the "configured limit" now names its key
- F-0040 Story-to-test gate: a Task's pull request must tick an acce… — clean — reads the Task's pull request and the record's Story lines; nothing single-valued
- F-0041 Small-Task trains: size class from the footprint, one train… — reworded — `product:size_classes`, `product:conventions.branch_prefixes.batch`, `product:ci.workflow` — thresholds, train branch prefix and the CI run each name the product key they read
- F-0042 Credentials as a rule: a daily check that every signed-in t… — reworded — `product:credentials` (new key) — the per-product provider list is data under a new key; the probe set is shared code
- F-0043 CI waste targets on the scorecard, and a week over target f… — reworded — `product:ci.provider`, `product:ci.budgets.cancelled_minutes_pct` (new key), `product:ci.budgets.red_rate_pct` (new key) — the CI provider is the product's; the two targets get keys
- F-0044 The retro's first line: features on production per week, an… — clean — per product with a cross-product total only in the operator view, as the isolation table asks
- F-0045 The session ledger: one durable line per session, joined to… — clean — one stream per product, every line carrying its product; nothing single-valued
- F-0046 The conflicts pass: supersession enforced, same-subject rul… — reworded — `product:backlog_dir` — the pass runs over core rules plus the product's own and writes into that product's record
- F-0047 The factory's self-improvement loop: measure, diagnose, act… — reworded — `product:improvement.thresholds` (new key) — the per-product thresholds get their key
- F-0048 The five factory slowdowns, fixed — reworded — `product:ci.budgets`, `product:slowdowns.candidates` (new key) — candidate list and thresholds each name the key they are read from
- F-0049 The factory floor is always clean: an owner, a registry and… — reworded — `config:paths.state_dir`, `config:paths.log_dir`, `product:conventions.briefs_dir`, `product:floor.ttls` (new key) — scratch, logs and briefs read their configured directories; the TTLs are per product
- F-0050 The product improvement loop: the factory proposes product… — reworded — `product:improvement.measurements` (new key), `product:probe.identity` (new key), `product:approvals` — measurements, probe identity and guardrails each name their key
- F-0051 The production verification probe: a real customer journey… — reworded — `product:app_host`, `product:deploy_sha.provider`, `product:ci.labels`, `product:probe.identity` (new key), `product:probe.mailbox` (new key) — production host, deploy detection, runner labels, probe identity and mailbox each name their key
- F-0052 Budget enforcement per Epic — clean — the budget is a per-record typed field, so it is per product by construction
- F-0053 Reviews return checks: every review ends in a machine-reada… — reworded — `product:conventions.review_pattern` — the review path names its key
- F-0054 Failure classification before any relaunch: credential, quo… — reworded — `config:worker_pool.accounts`, `config:quota_guards.stop` — "another account of the shared pool" and the quota band name their keys
- F-0055 A frozen eval set for the factory's judging tools — clean — the eval set is factory code; its score is one metrics line per product; nothing single-valued
- F-0056 Memory to code, first pass: what a session has to remember… — clean — the source is the controlling session's memory, an operator-level thing; the behaviours become code in ASF
- F-0057 Memory to code, second pass — clean — same as the first pass: operator-level memory into code
- F-0058 Memory to code, third pass — clean — same as the first pass: operator-level memory into code
- F-0059 The factory's tools are product-agnostic: one config file p… — reworded — `product:product`, `product:repo_dir` — "the product repository" is the product's own repo_dir; the one-file-per-product rule names product:product
- F-0060 A mechanical correctness pass before the human-standard rev… — reworded — `product:size_classes` — the size class is the product's
- F-0061 Hooks as rule enforcement inside every worker session — reworded — `product:main`, `product:backlog_dir` — the main branch and the record are the product's
- F-0062 The launch path: skills as procedures, role agents with res… — reworded — `config:worker_pool.roles` (new key), `product:main` — per-role model/effort gets a key; the main branch is the product's
- F-0063 Relaunch from the branch, not from zero — reworded — `product:main` — the main branch is the product's
- F-0064 Events at the source: every tool appends its own event, and… — reworded — `config:paths.log_dir`, `config:worker_pool.prices` (new key) — the rendered log lives under the configured log_dir; the price table gets a key
- F-0065 Spike: a sandboxed shell for worker sessions — clean — a spike over which tools break under a sandbox, per role; it names no product-specific fact
- F-0066 A progress heartbeat: no progress for twenty minutes is a s… — reworded — `product:stage_limits.task_active`, `product:stage_limits.no_progress` (new key) — the silence limit is the product's task_active; the progress limit gets a key
- F-0067 Clean floor, re-scoped: the sweep, the registry and the too… — reworded — `config:paths.state_dir`, `product:floor.ttls` (new key) — TTLs are per product; the sweep works under the configured state dir
- F-0068 Spike: one Feature through a hosted agent runtime, compared… — reworded — `config:worker_pool.accounts`, `config:quota_guards.stop` — the account pool and the quota guard name their keys
- F-0069 Security checks by code: a review pass on sensitive paths,… — reworded — `product:security_paths` (new key), `product:repo_slug`, `product:ci.labels` — sensitive paths, alert source repo and CI boxes each name the product key they read
- F-0070 A CI step-silence rule: a job with no new step for ten minu… — reworded — `product:ci.provider`, `product:ci.budgets.step_silence_minutes` (new key) — the provider is the product's; the ten-minute threshold gets a key
- B-0008 A tick that staged everything in a shared checkout committe… — clean — an incident; the tick's own clone is per product, and the Fix section is outside what a re-word may touch
- B-0009 A hand harvest reaped a worktree before its fast-forward ha… — clean — an incident; harvest reaps only after a successful push — nothing single-valued
- B-0010 A harvest reaped a worktree whose job was still running — clean — an incident; the finished-line and process-id check is per job — nothing single-valued
- B-0011 Tests run under the pre-commit hook inherit the git environ… — clean — an incident about the hook's inherited git environment; no product, repo or path is assumed
- B-0012 A fixture test reads the job's id range from the environmen… — clean — an incident about a fixture reading the job's id range; the range is per job

## New keys

- `config:quota_guards.controller_warn` — F-0035 — the weekly-window percentage of the controlling account past which the chore check runs (60 by default)
- `config:worker_pool.prices` — F-0064 — the dated price table that turns a session's four token dimensions into money
- `config:worker_pool.roles` — F-0062 — model, effort and permission mode per worker role
- `product:ci.budgets.cancelled_minutes_pct` — F-0043 — target for cancelled runner minutes as a share of all (15 by default)
- `product:ci.budgets.red_rate_pct` — F-0043 — target for the CI red rate (15 by default)
- `product:ci.budgets.step_silence_minutes` — F-0070 — how long a CI job's newest started step may go without a completed one before the job is stalled (ten by default)
- `product:ci.factory_only_paths` — F-0038 — path globs a factory-only push may touch
- `product:credentials` — F-0042 — the providers this product needs signed in, one entry each
- `product:floor.ttls` — F-0049, F-0067 — per-class time-to-live for ephemeral things: worktrees, hold branches, scratch, briefs, logs
- `product:improvement.measurements` — F-0050 — the measurements the product improvement loop reads, with their streams
- `product:improvement.thresholds` — F-0047 — thresholds the factory's self-improvement loop reads for this product
- `product:probe.identity` — F-0050, F-0051 — the development-only identity the production probe signs in as
- `product:probe.mailbox` — F-0051 — the mailbox the probe reads its sign-in code from
- `product:security_paths` — F-0069 — path classes whose changes need the security review pass
- `product:slowdowns.candidates` — F-0048 — the candidate causes the slowdown pass measures for this product
- `product:stage_limits.no_progress` — F-0066 — how long a job may show no new commit, file or tool class before it is stalled on progress (twenty minutes by default)
- `product:stage_limits.pr_stale_dirty` — F-0036 — how long an unreviewed pull request may stay dirty before it is closed (seven days by default)
- `product:stage_limits.writer_dead` — F-0039 — how stale a writer session's heartbeat may be before a fixer is launched (six minutes by default)
