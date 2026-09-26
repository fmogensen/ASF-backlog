# Local worker sessions can't authenticate git pushes (keychain unavailable); route via factory credentials

2026-09-26 botseon: 4 local worker sessions (3 on one account, 1 on another) failed to push with `could not read Username for 'https://github.com': Device not configured` (osxkeychain helper unavailable to the launched process; SSH has no key) — adjudicate-f-0007-correction, correct-f-0003-correction, correct-f-0035-correction, review-pr-0599. The factory's own publish then pushes for them, so work isn't lost, but the session reports NEEDS OPERATOR and the item gets an extra round.

Fix (no credentials created or printed by ASF): make worker git pushes use the same credential route the factory's own pushes use (e.g. set GIT_ASKPASS / credential.helper to `gh auth git-credential` in the worker env when gh is authenticated for the operator), or have workers not push at all and let publish push (the session commits; harvest publishes). Doctor row: "worker push auth" probes `git ls-remote` + a push --dry-run from a worker env per account and reports which fail. Test with a fake helper.

## Question
This reads as a defect. A Bug carries a signature — add signature: <the failing test or error line>; or an ## Acceptance list if it is new work.
