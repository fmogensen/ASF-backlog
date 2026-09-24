# A product declares a worktree setup command; the spawner runs it in every fresh worktree

## What is wanted
A product declares a worktree setup command in its yaml (e.g. an offline, frozen-lockfile dependency
install). The spawner runs it once in every fresh worktree before the session starts, records its
duration, and refuses to hand over a worktree where it failed. It never links the main checkout's
dependency directory into a worktree.

## Evidence (reported by the first customer install)
Worker worktrees had no dependencies. Linking the main checkout's dependency directory made the
package manager refuse every script as an unsafe modules dir, so the push hook died at its first
step; skipping the check left per-package dependencies missing and typecheck reported a blind
"0 errors". A real offline install took 6 s from the local store and made every hook step pass.
Today `make_worktree()` runs `git worktree add` and nothing else.

## Fix direction
`conventions.worktree_setup` (a command, optional); `spawn.make_worktree` runs it for a new
worktree (and after a rebase that changed the lockfile), with a timeout, output to the job log;
failure is a spawn error naming the command. `asf doctor` reports a product without one whose repo
has a lockfile.

## Test
Fixture product whose setup command writes a marker file: a fresh worktree has the marker before
the session starts; a failing command refuses the spawn with its first stderr line; a reused
worktree does not re-run it unless the lockfile changed.
