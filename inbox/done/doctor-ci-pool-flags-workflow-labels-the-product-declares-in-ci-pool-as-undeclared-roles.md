→ F-0304

# doctor ci-pool flags workflow labels the product declares in ci.pool as undeclared roles

Parent: E-0001

doctor's ci-pool check flags workflow `runs-on` labels (light, class-pr-heavy, hetzner-heavy) as undeclared roles, even though the product declares them in its `ci.pool` table. Seen 2026-10-08 on botseon.

## Acceptance
- The role list doctor checks against includes every label the product declares in `ci.pool` (roles and per-box labels), plus GitHub's built-in labels. A label in neither is still flagged. Tested with a pool fixture declaring the three labels.
