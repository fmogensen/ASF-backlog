# The merge train's starvation guard is dead now that item PRs never use auto-merge
parent: E-0006

Type: Bug

The merge train's starvation guard is dead. `train_key` (asf/kernel/decide.py) puts a PR first only when `pr.auto_merge and pr.auto_merge_at` is older than `config.landing_max_wait_h`. Since B-218952 the kernel never enables auto-merge on an item PR, so no PR is ever starved-first and a low-rank behind PR can wait indefinitely on a strict trunk. `hold_risky` also sorts its lane by `auto_merge`.

## Acceptance
- [ ] The wait is measured from when the approve first held, not from `auto_merge_at`.
- [ ] A test: an approved behind PR waiting past `landing_max_wait_h` is updated first with auto-merge off.
- [ ] `hold_risky` no longer orders by `auto_merge`.
