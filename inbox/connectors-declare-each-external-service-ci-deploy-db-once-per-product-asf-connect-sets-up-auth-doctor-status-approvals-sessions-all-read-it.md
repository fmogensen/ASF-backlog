# Connectors: declare each external service (CI, deploy, db, …) once per product — asf connect sets up auth, doctor/status/approvals/sessions all read it

Operator, 2026-09-24: "Think generic and create connectors or configs for external things that need auth or similar … should be easy to set up on asf."

Today:
- asf/doctor.py hard-codes one list of vendor CLIs (_CLI_TOOLS: gh, gcloud, az, aws, flyctl, vercel) and only checks whether each is logged in.
- What a product *uses* each service for (CI, deploy, database, preview) lives nowhere as data. So the Prod and Runners rows need separate hand-set keys (ci.runner_org, deploy_sha.workflow), and the approvals matrix can't tell a deploy from a read.
- Worker sessions run on isolated account config dirs and don't inherit the operator's logins.
- The first customer uses external CI, a hosting platform and a hosted database, and hit each of these gaps.

Want: **connectors**, declared per product, driven by one generic model.

```yaml
connectors:
  <name>:                       # free name, e.g. host, db, ci
    kind: cli | token | oauth
    template: <catalogue entry> # optional: a shipped template fills the defaults below
    check: [<argv>]             # prints the identity; rc 0 = authenticated
    login: [<argv>]             # what the operator runs once, interactively
    secret: keychain:<service> | env:<VAR> | file:<path>   # never in the record or repo
    provides: [ci, deploy, db, preview, secrets]           # capabilities other parts read
    actions:                    # command patterns → approval class
      deploy: {match: "<regex>", class: touch_production}
      migrate: {match: "<regex>", class: touch_customer_data}
    sessions: inherit | none    # whether worker sessions get this auth
```

Pieces:
1. **Catalogue:** templates for common services, shipped as data files (not code), overridable per product. Adding a service is a data change.
2. **`asf connect <name> --product P`:** guided, idempotent setup.
   - Is the tool installed? If not, print the install line.
   - Is it logged in? If not, print the exact `! <login>` line for the operator. The operator never edits a file.
   - Verify the check, write the connector into the product yaml (the one place ASF writes operator config, with a backup).
   - Offer `sessions: inherit`.
3. **`asf connectors --product P`:** one table: connector, provides, identity, ok/expired/missing, last checked.
4. **doctor:** its rows come from the product's connectors, replacing the hard-coded `_CLI_TOOLS` list. A connector that is required and unauthenticated is RED, with the one command that fixes it.
5. **Status rows:** Prod reads the connector that `provides: deploy`, Runners the one that `provides: ci`. The old keys stay as aliases.
6. **Approvals:** a connector's `actions` feed the approvals matrix, so a deploy through any connector is `touch_production` with no per-vendor code.
7. **Sessions:** with `sessions: inherit`, a worker session gets that connector's auth, scoped to the connector:
   - an env var, or a config dir copied or linked into the account's isolated dir
   - the redaction gate knows each connector's secret shape, so a secret never reaches a commit, record or log
8. **Expiry:** the health step re-runs the checks every tick (cheap, cached for N minutes). An expired login is one NEEDS OPERATOR line naming the login command; it doesn't fail a session halfway.

Genericity: ASF code names no vendor. Vendors appear only in catalogue data, and check_generic stays green.

Tests:
- templating
- connect: missing tool / not logged in / ok
- the doctor rows come from connectors
- an action pattern triggers its approval class
- an inherited secret is redacted in a session's commit
- the Prod row reads the deploy connector
