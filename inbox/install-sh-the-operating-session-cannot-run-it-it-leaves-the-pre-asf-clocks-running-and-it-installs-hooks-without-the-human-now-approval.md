# install.sh: the operating session cannot run it, it leaves the pre-ASF clocks running, and it installs hooks without the human-now approval

Found while installing ASF for botseon (first customer) on 2026-09-24, following the README install path.

1. The install cannot be done by the Claude session that operates the product. In Claude Code auto mode, `pipx install --suffix=-live "git+https://github.com/fmogensen/ASF.git@<sha>"` is refused by the auto-mode classifier ([Auto-Mode Bypass]). The session also cannot add an allow rule for it ([Self-Modification]). So `curl … install.sh | bash` wraps a step the session is not allowed to run. Only the operator, in a terminal on the Mac, can do step 1, and that isn't possible from a phone or the web. The docs should say so ("the operator runs install.sh; the session takes over after"), or the install should split into an operator step (pipx) and a session step (hooks, scheduler, doctor).

2. install.sh installs the product's clocks but never retires a product's pre-ASF scheduler. botseon still runs com.nordio.factory-dispatch/-batch/-daily, and its botseon.yaml wave step runs the same factory-cron.sh as a command step. Right after install.sh, two clocks drive the same work every 10 minutes. The doctor's `one-factory` row passed anyway. Expected: install.sh (or `scheduler install`) refuses, or names the jobs to retire, while a known pre-ASF job for the product is loaded, and `one-factory` goes RED in that state.

3. Hooks install (step 3) touches the product's tracked git hooks. botseon.yaml's approvals put that class under human-now, but install.sh runs it without asking.

## Question
This reads as a defect. A Bug carries a signature — add signature: <the failing test or error line>; or an ## Acceptance list if it is new work.
