→ B-0116

# Spec/plan approval ignores review_pattern: one review-file contract for every kind
signature: spec and plan approval reads only <slug>-review-r<n>.md, so a review saved under conventions.review_pattern never approves the spec
parent: E-0001
severity: S2

Spec and plan approval ignores the product's review naming. Approval of a spec/plan reads only `<slug>-review-r<n>.md` plus the first verdict word (asf/evidence/evidence.py rx_review ~243, ~487). `conventions.review_pattern` (default `{reviews_dir}/{n}-{slug}.md`) and the `verdict:` line apply only to code PRs (asf/harvest/harvest.py ~1122-1142). So a spec review saved under the product's configured pattern, or even under the default pattern, never approves the spec. Reported by the first customer's review of the user guide.

Fix: one review-file contract for every kind. Spec, plan and code reviews are all found through `conventions.review_pattern`, and all read the same verdict syntax (`verdict: approved`, with the legacy first-word form accepted). The review brief writes exactly that. Migration: keep matching `<slug>-review-r<n>.md` as a fallback.

Tests: a spec review under the default pattern with `verdict: approved` → spec-approved; the legacy name still works; the code path is unchanged.
