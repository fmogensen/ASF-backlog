→ F-0112

# Products update themselves to a new ASF release: upgrade: auto in the tick, with rollback on a red doctor
parent: E-0001

parent: E-0001

Operator, 2026-09-24: "when a new version of asf is available, let the botseon session know, so that it can update automatically". A product's operating Claude Code session cannot reinstall ASF itself (auto mode refuses `pipx install`; see F-0108), but the product's own launchd tick runs outside Claude Code. So the update belongs in the tick, not in a session.

## Acceptance
- [ ] Product config `upgrade: auto | notify | off` (default notify). The tick compares the installed release (package version plus the pinned sha, recorded at install time) with the newest release tag on the ASF repo, reading tags at most once an hour.
- [ ] `auto`: when a newer tag exists and no session of the product is mid-landing, the tick reinstalls the pinned install at that tag (`pipx install --force git+…@<tag>`), re-renders the product's clocks (`scheduler install`), runs `doctor`, and logs `upgrade: <old> → <new>`. If the doctor goes RED, it reinstalls the previous tag and files a Bug.
- [ ] `notify`: the tick prints `UPGRADE AVAILABLE <old> → <new>` once per tag, in its log and in the tick digest (B-0087), so the operating session sees it.
- [ ] Generic: no product names. Works for any product installed with tools/install.sh. Tests cover version comparison, the auto path with rollback, and notify-once.
