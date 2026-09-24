→ F-0118

# Every product session shows its factory status each tick: a statusLine fed by the tick's snapshot

Every Claude Code session working on a product shows that product's factory status, updated each tick. The operator runs no `/loop` and no central controller session. Operator 2026-09-24: "each session runs asf for itself ... no central controller"; the durable fix was chosen over per-session `/loop 5m /asf:status`, which a session's auto-mode refuses to arm when a peer asks and which dies with the session.

Mechanism (from the Claude Code docs, statusline.md): the `statusLine` setting runs a command, shows its stdout (multiple lines allowed), and re-runs it every `refreshInterval` seconds and on events. Plugins can't set it; a settings file can.

1. The tick writes a status snapshot at its end: `state/<product>/status.txt`, the same rows as `asf status`, compact. Writes are atomic (tmp + rename).
2. `asf status --line --product P` prints that snapshot, plus its age ("tick 3m ago"). It only reads a file, so it's fast; it never recomputes. It prints a single line "no tick yet" when the snapshot is missing, and marks it "STALE" when the snapshot is older than 2 clock intervals.
3. `asf hooks install --product P` (which tools/install.sh already runs) merges `statusLine: {type: command, command: "<asf> status --line --product P", refreshInterval: 60}` into the product repo's untracked `.claude/settings.local.json`.
   - A statusLine that isn't ASF's is left alone: NEEDS OPERATOR, naming the line to add. The same rule as the git hooks.
   - Running the install twice changes nothing.
4. doctor gets a row: statusline ok/missing.

Cross-session messages aren't used: the receiving session holds them for approval. UserPromptSubmit additionalContext isn't used either: it only fires when the operator types.

Tests:
- the tick writes the snapshot atomically
- `status --line` shows the age and STALE
- hooks install merges the statusLine, twice changes nothing, and a foreign statusLine is kept with a NEEDS OPERATOR line
- doctor has the row
