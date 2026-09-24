→ F-0111

# hooks install from asf-live tells the operator to wire the dev install's path into the product's git hooks

On 2026-09-24, botseon ran `asf-live hooks install --product botseon` from the pinned asf-live install (pipx suffix -live, beside a dev `asf`). It refused, which is correct: botseon's pre-push hook is foreign. But it told the operator to add `"/Users/frank/.local/bin/asf" redact --pre-push --product botseon`. That path is the editable dev install, not asf-live, so following it would tie botseon's git hooks to the dev checkout, which is exactly what the live install exists to avoid.
Expected: the hook line names the running install's own entry point, the way scheduler install uses sys.executable. The doctor also flags a hook that points at an editable install.
