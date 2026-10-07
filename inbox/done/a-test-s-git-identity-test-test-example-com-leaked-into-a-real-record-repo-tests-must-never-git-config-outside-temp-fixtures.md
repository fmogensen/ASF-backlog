→ F-0281

# A test's git identity (Test <test@example.com>) leaked into a real record repo; tests must never git config outside temp fixtures

Parent: E-0001
severity: S2

A test leaked its git identity into a real product record repo. On 2026-10-07, ~/Code/botseon/backlog/.git/config carried a repo-local `user.name=Test`, `user.email=test@example.com`, the exact pair ASF's tests set: tests/test_gitfixture.py:27-28, tests/test_harvest.py:167-168/224-225/538, and the tests/test_lifecycle.py fixtures. Record commits were authored "Test" from 08:08 local onward. That test environment also lacked the redaction name lists, so titles were written back unscrubbed and every record push was refused (F-0273). A test or session ran `git config` with a cwd that resolved to a real repo, not a temp fixture.

## Acceptance
- One shared test fixture creates every git repo used in tests, under a temp dir, with identity passed by env (`GIT_AUTHOR_*`/`GIT_COMMITTER_*`), never `git config` in the repo. A grep test fails on any `git config user.` in tests/ outside that fixture.
- A guard in that fixture, and in any helper that runs `git config`, refuses a path outside the test temp root and outside a path the test created. Tested.
- `asf doctor` flags a product repo or record whose local git config sets a user.name/user.email that differs from the configured factory identity. Tested.
