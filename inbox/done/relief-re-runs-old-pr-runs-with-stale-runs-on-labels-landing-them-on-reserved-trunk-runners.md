→ F-0210

# Relief re-runs old PR runs with stale runs-on labels, landing them on reserved trunk runners
parent: E-0002

2026-09-27: CI relief re-runs (`gh run rerun`) of PR runs created before the product changed its runs-on labels (botseon #849 added class-pr-heavy for PR jobs) re-run with the OLD job labels (self-hosted,hetzner-heavy), so they land on runners reserved for the trunk (reserve-main) and starve main's required jobs; they looked like "busy with no job" to scans of recent runs. Fix: when a relief/queue re-run target's workflow definition at the run's head differs from the current trunk's runs-on for its jobs (or the run is older than the last workflow change), don't `rerun` — instead trigger a fresh run (re-request checks / push-free dispatch) so current labels apply; or skip and let the PR's next push run it. Test: pre-change run in relief → not re-run with stale labels.
