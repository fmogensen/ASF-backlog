# asf pr-hygiene --lanes: a machine-readable listing product rule checks can call

Product rule checks sometimes need to know which PRs ASF's PR-hygiene pass already owns: its lanes (conflicting, stale, awaiting rebase, and so on). Without that, a product rule like "no PR sits conflicting" flags PRs that the hygiene pass is already handling. Today `asf pr-hygiene` has no listing output, so a product that retires its legacy tooling loses that signal. Reported by the first customer install.

Want: `asf pr-hygiene --product P --lanes` prints a machine-readable listing: one JSON object per line, {pr, branch, lane, since, action}. It reads the state the prs step already caches (pr-hygiene.json) and makes no network call. It exits 0 with no output when there's nothing.

Document it in the rule-check contract as the supported way for a product check to ask "does ASF own this PR?".

Test: cached state with two PRs in two lanes → two JSON lines; empty cache → no output, rc 0.
