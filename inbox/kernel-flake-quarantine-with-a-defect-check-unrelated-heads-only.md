# Kernel flake quarantine with a defect check (unrelated heads only)

kernel.flake: a failure signature red on >= sources_min unrelated PR heads within window_h becomes a quarantine entry plus a DoR-ready Bug (fix session), capped at max_h; a signature whose sources share a changed file is a defect, not a flake. Acceptance: tests for flake vs shared-file defect and the cap.

parent: F-0334 (ASF 0.3). Generic: no product named in code.

## Question
This reads as a defect. A Bug carries a signature — add signature: <the failing test or error line>, or paste that line into the body (an `Error:` line or a `file:line › test` line is read as one); or an ## Acceptance list if it is new work.
