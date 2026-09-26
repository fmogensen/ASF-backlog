→ F-0161

# Wave launch costs ~55 s per session: 5 launches make most of a 242 s wave

Evidence 2026-09-26 16:31-16:35 botseon tick: demand.json written 16:31:46; briefs review-b-1381 16:32:32, review-t-0360 16:33:46, review-t-0361 16:34:40 — ~55 s between consecutive launches; wave total 242.4 s with 5 launches (asf wave with 1 launch: 6-17 s).

Per-launch setup (worktree create/checkout, dependency install, brief build) runs serially inside the wave. Prove which sub-step dominates (time each in one launch), then run launch setup concurrently or move the install out of the wave. Complements the harvest gh/git fan-out card.
