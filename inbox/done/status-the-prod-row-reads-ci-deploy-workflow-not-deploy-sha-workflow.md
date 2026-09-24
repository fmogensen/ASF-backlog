→ B-0118

# status: the Prod row reads ci.deploy_workflow, not deploy_sha.workflow
signature: status Prod row reads ci.deploy_workflow and prints not configured on a product that sets deploy_sha.workflow
parent: E-0001
severity: S3

The status table's Prod row reads a key the product schema doesn't have.

A product that sets `deploy_sha.workflow: deploy-prod.yml` gets, in `/asf:status`:
  Prod │ — (not configured: ci.deploy_workflow)

`asf/env.py` (Product.conventions) documents `deploy_sha.workflow` as the source of
`conventions.deploy_workflow`, and docs/products.example.yaml puts the workflow there. The status
view asks for `ci.deploy_workflow` instead, so the Prod row is blank on a correctly configured product.

Expected: the status view reads `product.conventions['deploy_workflow']` (the same resolution
the evidence pass uses), and its "not configured" hint names `deploy_sha.workflow`.
Acceptance: a product with only `deploy_sha.workflow` set shows a prod sha in the status Prod row;
a test covers it.

## Related, found while writing the user guide (2026-09-24)

/asf:prod reads `deploy_sha.prod.source` / `deploy_sha.prod.workflow`, but docs/products.example.yaml documents a flat `deploy_sha: {workflow, …}` shape. So the status Prod row, the prod view and the example use three different shapes for one fact. Fix all three together: one documented `deploy_sha` schema (per environment if needed), read the same way by status, prod and doctor, and flagged by `asf config check` when a product uses another shape.
