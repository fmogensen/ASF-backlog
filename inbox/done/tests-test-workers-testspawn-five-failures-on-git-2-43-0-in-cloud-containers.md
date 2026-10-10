→ B-123640

# tests.test_workers TestSpawn: five failures on git 2.43.0 in cloud containers
signature: tests.test_workers TestSpawn: five failures on git 2.43.0 in cloud containers
parent: E-0001
severity: S2

Bug: five tests.test_workers TestSpawn cases fail on git 2.43.0 (the cloud containers' git) and pass on the factory host (34 tests OK on origin/main, 2026-10-10).

Cases: test_a_review_on_a_branch_behind_main_pushes_nothing, test_b0046_correct_row_spawns_on_the_held_branch_not_rebased_when_it_merges_clean, test_b0048_adjudicate_row_spawns_on_the_held_branch, test_b0051_ended_sessions_worktree_is_reused_not_refused, test_trunk_rebase_needed_names_only_what_a_rebase_clears.

Found by plans F-0279, F-0313 and F-0314 (cloud sessions asked whether they are trunk reds; they are not).

Acceptance: the five cases pass under git 2.43.0, or skip there with a stated reason, so cloud sessions stop seeing pre-existing reds.
