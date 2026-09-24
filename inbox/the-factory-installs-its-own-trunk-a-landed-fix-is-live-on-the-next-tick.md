# The factory installs its own trunk: a landed fix is live on the next tick

A fix that lands on the factory's own trunk is not live until someone runs `bash tools/install.sh <product> <sha>` for every product by hand; today each code-red hotfix (host-pressure hold, dry-run disk fill, one-factory) waited on that step, and botseon had to be told a pin each time. Want: the factory installs itself. When ASF's trunk moves past the installed commit and the new head's CI/gate is green, the asf tick installs it (pipx, pinned to that sha) once for the machine, runs `asf doctor` for every product, and rolls back to the previous sha if a doctor row that was green goes red. Every product's next tick then runs the new code; the tick log names the swap. A config switch (`self_update: trunk | tags | off`) picks trunk heads, release tags only, or nothing.

## Question
This reads as a defect. A Bug carries a signature — add signature: <the failing test or error line>; or an ## Acceptance list if it is new work.
