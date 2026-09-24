# Status Prod row names ci.deploy_workflow but the product schema rejects it

signature: "status Prod row asks for ci.deploy_workflow, but the product-file schema rejects it"
severity: S3

## Description
- `asf status` prints `Prod — (not configured: ci.deploy_workflow)`, and `views/status.py prod_cell` reads `product.ci['deploy_workflow']`.
- Adding `deploy_workflow: <file>.yml` under `ci:` in the product file makes `env.load_product` raise `ConfigError: line N: 'ci.deploy_workflow' is not a field of the product file`. A product that follows the status hint therefore breaks every step that loads it, the tick included.

## Repro
Add `deploy_workflow: deploy-prod.yml` under `ci:` in a product file, then run `env.load_product('<product>')`.

## Expected
The schema accepts `ci.deploy_workflow`, and the status hint names a key that loads. A test loads a product with every key that status names in a `not configured:` hint.
