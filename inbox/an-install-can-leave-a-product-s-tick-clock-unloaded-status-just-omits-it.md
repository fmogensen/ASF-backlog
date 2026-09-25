# An install can leave a product's tick clock unloaded; status just omits it

type: bug
severity: S2

An install can leave a product's tick clock unloaded. On 2026-09-25 at ~16:35, after an install of 205123f, `launchctl list` showed asf.asf.daily loaded but asf.asf.record-health-wave-prs-harvest missing, though its plist was still in ~/Library/LaunchAgents. The asf product silently stopped ticking. `asf status` showed it only as a missing name in the Cron cell, and `asf doctor` was not run by anyone. `asf scheduler install --product asf` restored it. Want: tools/install.sh verifies after its scheduler step that every clock the product declares is loaded, retries the bootstrap once, and fails the install loudly naming the missing label if it still isn't. The tick of any other product (or `asf status`) flags "clock <label> not loaded" as RED in the Cron row, instead of omitting it. Tests: a declared clock with a plist but no loaded job is reported not-loaded by status and doctor; install retries the bootstrap.
