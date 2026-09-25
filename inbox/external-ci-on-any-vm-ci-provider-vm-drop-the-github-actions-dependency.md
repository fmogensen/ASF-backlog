# External CI on any VM (ci.provider: vm) — drop the GitHub Actions dependency

Feature: an external-CI provider on any VM (`ci.provider: vm`).

Operator, 2026-09-25: "we're not using github, we're using ci with hetzner, which should be an asf option for external ci .. could be any vm connected really".

Today: botseon runs GitHub Actions workflows on self-hosted Hetzner runners (`ci.provider: github-actions`, runner_org). This ties us to GitHub's limits. The artifact storage quota currently fails every e2e job, and on top of that come the Actions queueing and runner registration.

Want: ASF drives CI itself on one or more connected machines, with no dependency on GitHub Actions.
- Config: `ci.provider: vm` plus `ci.hosts:` (ssh targets, labels, slots each). Also `ci.jobs:`, a named command per job and which are required, reusing landing_checks/required_checks. Artifacts are kept on the VM, or in a configured store, with a retention setting.
- The tick or harvest dispatches a job for a sha to a free host slot: checkout at the sha in a disposable workdir, run the command, collect the exit code and a log tail. Timeouts, a cancel when a newer push supersedes the run, and cleanup afterwards.
- Results feed the same place the harvest reads today: required checks decide red or green, and the rules for trunk-red, pending age and send-back all apply unchanged. Optionally post a commit status to the PR host so PRs show it. The PR host (GitHub) stays for code review and merging only.
- `asf capacity` counts the VM slots as ci capacity. `asf doctor` checks host reachability and free disk.
- Generic: any ssh-reachable machine, no product or vendor names in the code.

Acceptance: tests with a fake host (a local subprocess acting as ssh) covering dispatch, green/red/timeout/cancel, required-check judging and capacity. botseon can then switch from github-actions to vm by changing config alone.

## Question
This reads as a defect. A Bug carries a signature — add signature: <the failing test or error line>; or an ## Acceptance list if it is new work.
