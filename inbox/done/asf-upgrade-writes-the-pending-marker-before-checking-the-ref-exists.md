→ F-0162

# asf upgrade writes the pending marker before checking the ref exists

2026-09-26: `asf upgrade --ref <mistyped full sha>` (right 9-char prefix, garbage after) wrote state/upgrade-pending.json before verifying the ref exists; that marker would have parked every product's tick on a non-existent ref until its 20-minute TTL. Fix: resolve the ref (git rev-parse / ls-remote on the install source) before writing the pending marker; refuse unknown refs with a clear error. Test: an unknown ref writes no marker and exits non-zero.
