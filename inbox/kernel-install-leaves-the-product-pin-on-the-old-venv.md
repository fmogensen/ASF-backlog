# kernel install leaves the product pin on the old venv
parent: F-0346

Type: Bug

`asf kernel install --product <p>` switches the launchd jobs to the new kernel venv but leaves the product pin (state install.json sha/venv) on the old one. Workers then run a different asf than the kernel, and the console has to edit state by hand to align them.

Acceptance:
- kernel install writes the product pin to the venv it installs, with the old one recorded as previous, in the same step as the plists.
- --dry-run prints the pin change.
- Test: after install, the pin sha equals the kernel venv sha.
