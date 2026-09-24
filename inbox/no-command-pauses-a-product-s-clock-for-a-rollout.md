# No command pauses a product's clock for a rollout

The fix package's rollout (docs/guide/upgrading.md, step 1) says "pause the product's clock", but no `asf` command pauses one. The 2026-09-24 rollout had to `launchctl bootout` the jobs and `asf scheduler install` to resume, and the installer's own step 4 reloads the clocks and fires a tick mid-rollout. Want: `asf scheduler pause|resume --product <p>` (adapter-generic), the installer honouring a paused clock, and the guide naming the command.
