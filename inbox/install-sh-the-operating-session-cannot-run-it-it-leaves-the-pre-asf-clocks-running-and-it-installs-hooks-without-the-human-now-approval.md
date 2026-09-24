# install.sh: the operating session cannot run it, it leaves the pre-ASF clocks running, and it installs hooks without the human-now approval

Found while installing ASF for botseon (first customer) on 2026-09-24, following the README install path.

1. The install cannot be done by the Claude session that operates the product. In Claude Code auto mode, `pipx install --suffix=-live "git+https://github.com/fmogensen/ASF.git@<sha>"` is refused by the auto-mode classifier ([Auto-Mode Bypass]). The session also cannot add an allow rule for it ([Self-Modification]). So `curl … install.sh | bash` wraps a step the session is not allowed to run. Only the operator, in a terminal on the Mac, can do step 1, and that isn't possible from a phone or the web. The docs should say so ("the operator runs install.sh; the session takes over after"), or the install should split into an operator step (pipx) and a session step (hooks, scheduler, doctor).

2. (Botseon migration note, not an installer defect; the operator ruled 2026-09-24 that retiring pre-ASF clocks is the product's own migration step, out of F-0108.) Botseon retired com.nordio.factory-dispatch/-batch/-daily on 2026-09-24 before `asf-live scheduler install`; the plists are kept in ~/.ASF/state/botseon/retired/2026-09-24/.

3. Hooks install (step 3) touches the product's tracked git hooks. botseon.yaml's approvals put that class under human-now, but install.sh runs it without asking.

## Question
This reads as a defect. A Bug carries a signature — add signature: <the failing test or error line>; or an ## Acceptance list if it is new work.
