# install.sh step 1 failure exits with pipx's code and no NEEDS OPERATOR guidance

tools/install.sh step 1: when `pipx install` fails or the repo is unreachable, the script exits through `set -e` with pipx's own exit code and no NEEDS OPERATOR line, so the user gets no guidance. Reported by the first customer's review of the user guide.

Fix: wrap step 1 so any failure prints `install: NEEDS OPERATOR: pipx install of <ref> failed — <the last stderr line>; check the ref and network` and exits 2. Test with a stub pipx that fails.

## Question
This reads as a defect. A Bug carries a signature — add signature: <the failing test or error line>; or an ## Acceptance list if it is new work.
