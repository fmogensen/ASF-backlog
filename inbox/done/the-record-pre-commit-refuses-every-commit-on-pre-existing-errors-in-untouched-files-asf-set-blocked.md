→ B-0132

# The record pre-commit refuses every commit on pre-existing errors in untouched files (asf set blocked)

type: bug
severity: S2

A record's pre-commit (`asf check --product <p>`) fails on errors anywhere in the record, not just in the staged change. On 2026-09-25 botseon's record had 8 pre-existing "bare decision reference" errors in tasks T-0279…T-0331 (and a stale index.json until `asf index` ran). So every operator or session commit to the record was refused, `asf set --product botseon B-1372 blockedBy=…` included (CalledProcessError on its commit). The tick's own record commits went through meanwhile. A person could not put an item on hold while a paid review loop kept relaunching it. Want: in pre-commit mode, `asf check` fails only on errors in the files being committed (and on errors the staged change introduces elsewhere). Errors already present in untouched files print as warnings with a count ("8 pre-existing errors in files not staged"). The groom files one Bug per error class so they get cleaned up. `asf set` keeps index.json fresh as part of its own commit. Tests: a record with a pre-existing error in an untouched file accepts an `asf set` on another card; an error introduced in the staged file is still refused.
