# approvals: touch_security path match fires on read-only commands (grep/sed -n of .githooks/*)

The touch_security path recogniser (paths: .githooks/*, …) fires on a Bash command that only READS a
listed path. Seen: a fix-bug session ran `grep -n … .githooks/pre-push; sed -n 1,30p …` to look up a
check, and got a human-now touch_security hold. Its branch never changed .githooks/ at all, so the
operator had to drop the hold by hand.

Same family as the earlier core.hooksPath read fix (06bcc00), but for the path list.
Expected: a path-class match on Bash counts only writes — redirection into the path, mv/cp/rm/chmod/
tee/sed -i/git add of it, or an Edit/Write tool call — not read-only commands (cat, grep, sed -n, head,
ls, git diff/show/log).
Acceptance: approvals test — `grep x .githooks/pre-push` and `sed -n 1,5p .githooks/pre-push` raise no
hold; `echo x >> .githooks/pre-push` and an Edit of it still do.
