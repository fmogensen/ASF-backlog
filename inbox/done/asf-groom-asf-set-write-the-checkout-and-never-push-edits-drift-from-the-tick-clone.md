→ B-0102

# asf groom / asf set write the checkout and never push — edits drift from the tick clone
signature: asf groom and asf set write backlog_dir and make no commit - the tick clone resets to origin every tick and the edits never reach it
parent: E-0001
severity: S2

`asf groom` and `asf set` write the operator's record checkout (backlog_dir) and never commit or push. The tick works in its own clone, which resets to origin every tick, so these edits never reach it. They silently drift and are lost or conflict later. `asf groom --apply` also reads only groom files dated before today. Reported by the first customer's review of the user guide.

Fix: every command that writes the record (groom, groom --apply, set, and any other writer) commits and pushes its own change, the way `asf inbox` does (retry on a non-fast-forward, fail loudly on a conflict). `groom --apply` accepts today's answers file. Test: each writer → one commit on origin, and the next tick sees it.
