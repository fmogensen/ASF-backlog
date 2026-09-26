# Plan preflight across open branches: refuse a reserved range another open branch already holds

2026-09-26 botseon: #857 (F-0113's plan) landed a migration band 0289-0290 on main while open branch cloud/T-0359 (PR #844) already reserved 0289 — the planner booked a range without seeing open branches' reservations; T-0359 now needs a re-book and F-0112's floor moved.

Generic ASF feature: a product-configured `plan.preflight` command (e.g. botseon: `node scripts/bands.mjs check`) that the plan/land step runs against the union of origin/main and every open lane branch (cloud/*, PR heads) — or the product exposes a "reservations" command whose output ASF merges across branches — and refuses a plan (or its landing) whose reserved ranges overlap any open branch's. Decision is deterministic (code over LLM); the planner's brief also lists the open branches' reservations up front. Test: a plan booking a range an open branch holds is held with the clashing branch named.
