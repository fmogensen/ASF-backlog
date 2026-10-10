# Bug: a cloud routine is polled under the ambient CLI login when its account does not resolve

Bug (defect): a cloud routine is polled/ended under the ambient CLI login when its account does not resolve

Type: bug. Botseon's swarm reported the same defect class ("default" meant whatever login the CLI currently has; after an account switch, polls ran under another login: HTTP 400 "Environment env_… does not exist" and "no run status" for sessions that were still running). Fixed there in scripts/swarm (commits a5ea65c34, d55f8f05a, e91fa9672 for reference). ASF's main path is sound — the session record stores a named pool account, each with its own config_dir and per-account environment id — but two fallbacks still reach the ambient login.

## Problem (evidence, origin/main)

- asf/workers/remote.py:680-686 `_account(run)` returns None when the run's recorded account is no longer in `worker_pool.accounts` (renamed, removed, moved to a history-only list while its routine is live).
- asf/workers/remote.py:550, :606, :652, :671 then build `TriggerClient(None)`; asf/workers/remote.py:182-193 `helper_env(None)` sets no `CLAUDE_CONFIG_DIR`, so the helper `claude -p` runs on whatever login the operator's default config dir holds at that moment — which an account switch changes.
- The same happens for a lane account configured without `config_dir`: asf/workers/remote.py:190 only sets the dir when present, and asf/workers/cloud.py `config_problems` (:236-) does not require `config_dir` for a cloud-lane account, while the environment id is pinned per account name (`environment_for`, remote.py:396-400).
- remote.py:551-553 a HelperError on get keeps the cached `last_run`, so the wrong-login poll is silent: the run reads its stale status until the timeout instead of being reported unreadable.

## Failure scenario

A routine is created on lane account X; the operator drops X from `worker_pool.accounts` (or X has no config_dir) and switches the CLI's default login. Every later `last_run`/`diagnose`/`retire` call for that run asks the routine API as a different account, which cannot see X's trigger (404/400); the run is never seen ending, holds a seat and its item stays blocked while the session runs on; `retire` cannot disable the trigger.

## Invariant / fix

- Poll, diagnose, run-log and retire always run on the login that created the routine: store the account's config_dir (or a stable login identity) on the cloud session record at launch and use it when the name no longer resolves.
- Never fall back to the ambient login: `TriggerClient(None)` / `helper_env(None)` for a cloud run is refused with a named reason (run unreadable: account <name> unknown), surfaced on status, not silently cached.
- Config check: a `role: cloud` / `cloud.accounts` account without `config_dir` is a config problem for runtime claude-remote.
- On a miss for a run whose account does not resolve, try each configured lane account's login once and adopt (record) the one that sees the trigger.

## Acceptance (tests)

- tests/test_remote.py: a run whose account is not in config is polled with the config_dir stored on its record; `helper_env` never runs without `CLAUDE_CONFIG_DIR` for a cloud run.
- tests/test_remote.py: with no stored dir and no resolvable account, `last_run` tries each lane account's client, adopts the one whose `get` returns the trigger, and records that account on the run.
- tests/test_remote.py: if no login sees it, the run is reported unreadable (a named reason), not left on its cached status.
- tests/test_cloud.py: `config_problems` flags a cloud-lane account without `config_dir` under runtime claude-remote.

## Question
Which Epic is this under? No open Epic shares a title word with it.
