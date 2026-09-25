→ B-0127

# Every asf worker runs the full suite that harvest's gate already runs: 6 sessions = 6 full suites on the host

type: bug
severity: S2

On 2026-09-25 from about 10:30 to 11:10, load on the factory host held at 42-47 on 10 cores, and the host guard kept both products from launching (botseon sat at 0 sessions). The main source is our own workers: with 6-7 asf sessions running, 37 test-runner processes were alive, because each asf worker runs the full suite (`tools/run_tests.py --shards 4`) before pushing. For a product that lands by local gate (asf: fast-forward landing, no external CI), harvest's gate already runs the full suite on the combined head before anything lands. The worker's own full run duplicates that and multiplies it by the session count. Want: when a product's landing is gated by harvest's local full-suite gate, the brief tells workers to run only the targeted tests for the files they changed, and states that the harvest gate runs the full suite before landing. Nothing lands without the full suite passing; it just runs once per landing instead of once per session. Tests: a local-gate product's brief carries the rule; the harvest gate still runs the full suite.
