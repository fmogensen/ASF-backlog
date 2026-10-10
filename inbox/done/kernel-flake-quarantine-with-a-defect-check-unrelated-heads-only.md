→ F-0336

# Kernel flake quarantine with a defect check (unrelated heads only)
type: feature
parent: E-0003

kernel.flake: a failure signature red on >= sources_min unrelated PR heads within window_h becomes a quarantine entry plus a DoR-ready Bug (fix session), capped at max_h; a signature whose sources share a changed file is a defect, not a flake. Acceptance: tests for flake vs shared-file defect and the cap.

