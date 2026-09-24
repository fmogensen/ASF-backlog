# A push refused by the repo's pre-push hook loops as 'unpushed work' — carry the hook's output into the correction and route by it

When a session's `git push` is refused by the product repo's pre-push hook (a lint, a test, the redaction gate), the run ends "failed: unpushed work". The correction the next session gets says only "commit and push what you have", so it retries the same push and fails the same way, round after round. Seen on the first customer install: one Task looped this way for several rounds.

Want:
- The runtime and health detect a hook refusal: the session's last push attempt in its job log carries the hook's output and a non-zero exit.
- They classify it as `push refused by hook`, not as "unpushed work".
- The correction carries the hook's output tail (clipped, redacted), so the next session fixes the cause.
- After 2 identical refusals, route by content: a lint or test on files outside writes → widen_footprint; a redaction finding → a security hold; otherwise → the stalemate or adjudicate path. Never an endless loop.

Tests: a refusal is classified; the correction includes the tail; the routing after the second identical refusal.
