→ B-0135

# An install during a running tick tears it: groom ImportError from a half-swapped package

type: bug
severity: S3

An install that runs while a tick is running tears that tick: on 2026-09-25 at ~14:08 botseon's groom step failed with "ImportError: cannot import name 'as_list' from 'asf.record.core'" because `pipx install --force` replaced the package under the running tick (half the modules old, half new). The next tick was fine. Want: tools/install.sh takes each product's tick lock (or waits for running ticks to finish, bounded) before replacing the package, and the tick imports every step module up front at start, so a swap mid-tick cannot mix versions; the upgrading guide already warns about this, the installer should enforce it. Tests: install waits on a held tick lock.
