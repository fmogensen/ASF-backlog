# /asf:* in a product's own session shows another product: resolve the product from the working directory
parent: E-0001

parent: E-0001

First customer, 2026-09-24: in a product's own Claude Code session, /asf:next printed "0 rows". Nothing told the skill which product it was in: ASF_PRODUCT was not set, so it fell back to config.yaml's default_product (another product). `asf next --product <p>` showed that product's 6 rows. Every /asf:* view has the same trap on a machine with more than one product.

## Acceptance
- [ ] Resolve the product from the working directory before default_product: the product whose repo_dir or backlog_dir contains the cwd (the realpath, longest match wins). Order: --product, then $ASF_PRODUCT, then cwd match, then default_product.
- [ ] Every view's first line names the product it shows (`record: <path> · product <p>`), so a wrong fallback is visible.
- [ ] The installer's closing lines no longer ask for ASF_PRODUCT when the session runs in the product's repo.
- [ ] Tests for the resolution order with two products.
