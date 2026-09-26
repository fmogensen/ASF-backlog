# The approvals hook holds a read-only 'git config core.hooksPath' as a security write

A product's T-0157 was held touch_security (human-now) on a read-only command: `git config core.hooksPath && ls -la $(git config core.hooksPath 2>/dev/null || echo .git/hooks) 2>&1 | head -30`. Reading a git config key and listing a directory writes nothing. The hold sent T-0157 round STALEMATE → ADJUDICATE several times on 2026-09-25/26.

Wanted in code (asf/approvals.py): `git config <key>` with no value, and `--get`/`--list`/`-l`, is a read and never a security write. Only a set (`git config key value`, --add, --unset, --replace-all) of core.hooksPath and the like is one. Add tests for the read forms, and for the set form still holding.
