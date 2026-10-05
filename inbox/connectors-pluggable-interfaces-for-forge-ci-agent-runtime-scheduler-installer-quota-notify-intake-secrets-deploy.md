# Connectors: pluggable interfaces for forge, CI, agent runtime, scheduler, installer, quota, notify, intake, secrets, deploy

Operator 2026-10-06: "i like that ASF would have plug-ins or connectors to deal with external services. do we have that or need that, and if so what?"

Today: `asf plugin` is only the Claude Code skills plugin (UI). External services are called directly from many modules: `gh` from 13 files, the `claude` CLI from 5, `launchctl` from 4, `pipx` from 14. The only real seams are `worker_pool.quota_command` (a command hook), `cloud.runtime` (actions | claude-remote) and git hooks. A public user on GitLab, Linux/systemd, uv, or another agent runtime cannot run ASF without forking it.

Design: connectors = small Python interfaces (typing.Protocol) per external concern, one in-tree default implementation each, selected by config, extensible by third parties via Python entry points (`asf.connectors.<kind>`); every connector also accepts a "command" implementation (a shell command with a JSON contract, like quota_command) so users can integrate without writing Python.

Connector kinds (v1 = interface + the listed default; others later):
1. forge — PRs, reviews, checks, branches, merge (default: GitHub via gh; later GitLab, Bitbucket, plain git).
2. ci — runs, jobs, rerun/cancel/force-cancel, runners (default: GitHub Actions; later GitLab CI, Buildkite; runner provider sub-hook for self-hosted boxes).
3. runtime — start/stop/observe an agent session, heartbeat (default: Claude Code CLI local + claude-remote cloud; later other agent CLIs).
4. accounts/quota — list accounts, read usage, login state (default: none → all available; command form = today's quota_command).
5. scheduler — install/pause/resume clocks (default: launchd on macOS, systemd timers on Linux, cron fallback).
6. installer — install/upgrade/pin a version (default: pipx; uv option).
7. notify — alarms and operator questions out (default: tick log; later Slack, email, push, webhook).
8. intake — external issue trackers into the inbox (later: GitHub Issues, Linear, Jira).
9. secrets — resolve `auth_env` refs (default: file paths; later 1Password, OS keychain, env).
10. deploy/prod — "on prod" evidence (default: none; later Vercel, Fly, generic URL probe).

Acceptance: each v1 kind has a Protocol, a registry reading `connectors.<kind>: <name>` from config, the default implementation moved behind it with no behaviour change, a fake implementation used by tests, and a lint/test that fails if `gh`, `claude`, `launchctl` or `pipx` is invoked outside its connector module. `asf doctor` lists the active connector per kind. docs/guide gets "Writing a connector".
Release scope: kinds 1, 2, 3, 5 (plus 4 and 9 in command form) must exist as interfaces before public release; additional implementations are post-release.

## Question
This reads as a defect. A Bug carries a signature — add signature: <the failing test or error line>; or an ## Acceptance list if it is new work.
