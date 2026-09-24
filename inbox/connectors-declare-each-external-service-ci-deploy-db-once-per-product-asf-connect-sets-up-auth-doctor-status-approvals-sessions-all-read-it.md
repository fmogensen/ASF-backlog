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

## Also: LLM runners are connectors (operator, 2026-09-24)

"Because of multiple cux runners or different LLM runners, auth is a thing that is probably needed."

The worker pool is the biggest auth surface: several accounts per runtime, and possibly more than one runtime or LLM provider. Today each is configured by hand in `worker_pool.accounts` (config_dir, quota_command), outside any shared model. So runner accounts use the same connector model, at operator scope (config.yaml, shared by all products), not per product:

```yaml
connectors:
  <account>:
    kind: runner
    runtime: <runtime name>        # the runtime adapter that launches sessions
    config_dir: <isolated dir>     # the account's own home or config
    check: [<argv>]                # is this account logged in, and as whom
    login: [<argv>]                # the one interactive command to (re)login
    quota: [<argv>]                # prints {five_h_pct, seven_d_pct, ...}; feeds the bands
    models: [<model>, ...]         # what this account may run
    lane: worker | cloud
```

- `asf connect <account>` does for a runner what it does for a service: detect, print the login command, verify, write. Adding a worker account is one command.
- `asf connectors` shows every runner's identity, auth state, quota band and load, next to the service connectors.
- An account whose auth expired is taken out of the pool (like `stop`), with one NEEDS OPERATOR line naming its login command. A session is never launched onto it to fail.
- Service connectors with `sessions: inherit` are provisioned into every runner account's isolated config dir. That covers each account, and each runtime's own way of taking credentials, so a session on any account has the same external access.
- Runtime-neutral: a runner connector names its runtime adapter. A second LLM runtime is a new adapter plus catalogue data, not a change to the pool, quota or approvals code.

## First-customer requirements (service survey, 2026-09-24)

Key fact from the survey, verified in code: worker spawn (asf/workers/runtime.py build_env → asf/hermetic.py) sets only the runtime's config dir. HOME is not isolated, so every worker session inherits **every** CLI login the operator has: keychain and dot-dirs for the code host, hosting, database, payments, and cloud IAM. Env-file secrets reach no worker. So worker access is all-or-nothing today, and the only guard on a production write is an approval_signals regex. On the surveyed machine, a payments CLI's active context was LIVE and reachable by every worker.

The connector model must therefore include:
1. **Default-deny scoping.** Workers get an isolated HOME. Only connectors with `sessions: inherit` are provisioned into it, each with its own least-privilege credential where the service supports one (a deploy token, a test-mode key, a read-only role). "All logins via HOME" stops.
2. **Declared environments per connector** (`contexts: {dev: …, prod: …}`), with a doctor check that says which context is active. A worker may only ever see non-prod contexts unless the approvals matrix grants that class at `auto`. A live payments context visible to a worker is a doctor RED.
3. **Proof checks in doctor**: the CLI proof command for CLI connectors, and a key-exists check (name only, never the value) for env/keychain-only tokens.
4. **Loading rule**: connectors load secrets key by key from their declared source, never by sourcing a whole env file. Sourcing one leaked a deploy token twice.
5. **Per-account prerequisites** for runner connectors, e.g. an app grant the code host needs per LLM account for cloud sessions, checked per account.
6. **Headless-safe commands**: a connector can mark which of its CLI calls prompt when run headless and name the non-interactive equivalent (e.g. use the API form instead of a listing command that asks for approval).
7. **Leak response**: when the redaction gate sees a connector's secret shape in a transcript, commit or log, it files one NEEDS OPERATOR line naming the connector and "rotate".

Priority note: item 1 (scoping, isolated HOME) and item 2 (a prod or live context visible to workers) are safety defects in the current code. They should be built before the rest of the feature.

## Question
This reads as a defect. A Bug carries a signature — add signature: <the failing test or error line>; or an ## Acceptance list if it is new work.

## Key rotation as a connector capability (operator, 2026-09-24)

"Think about how key rotation could be part of asf if the provider allows it."

A connector can declare how its credential is rotated. ASF then runs rotation the same way every time: on a schedule, on a detected leak, or on demand.

```yaml
connectors:
  <name>:
    rotate:
      mode: api | guided | none          # what the provider allows
      create:  [<argv>]                  # api: mint a new credential; prints it on stdout only
      revoke:  [<argv with {old_id}>]    # api: revoke the old one
      max_age: 90d                       # scheduled rotation; doctor shows age vs max
      overlap: 10m                       # both keys valid while consumers switch
    consumers:                           # every place this secret lives
      - keychain:<service>
      - ci-secret:<NAME>                 # via the code-host connector's own action
      - app-secret:<app>/<NAME>          # via the hosting connector's action
      - env-file:<path>#<KEY>            # rewritten key by key, never sourced
      - accounts: all                    # each worker account's isolated store
```

`asf rotate <connector> --product P` runs one deterministic sequence, in code, never inside an LLM session:
1. **Create** the new credential (api), or for guided mode, print the provider console steps and read the new value from a hidden prompt or clipboard. The value never passes through chat, a transcript, argv or a log.
2. **Distribute** it to every consumer, each through that consumer's own connector action. The approvals class for each write applies.
3. **Verify** with the connector's proof check, using the new credential, from every place it's consumed (including one worker account).
4. **Revoke** the old credential after `overlap` (api), or for guided mode print the revoke step and wait for confirmation.
5. **Record** an audit line in the record: connector, rotated_at, who or what triggered it, and the fingerprints (a short hash of old and new, never the value). If distribution or verification fails at any consumer, roll back to the old credential (not yet revoked) and file NEEDS OPERATOR.

Triggers:
- **Schedule:** the health step flags `age > max_age` in doctor and the status table. Rotation then runs if the approvals matrix sets `rotate_secret` to auto, and otherwise raises one NEEDS OPERATOR line with the command.
- **Leak:** when the redaction gate sees this connector's secret shape in a commit, transcript or log, it files the finding and starts an emergency rotation, auto if approvals allow, else NEEDS OPERATOR with top priority. This is the case from the first customer, a runner-provider token pasted into chat.
- **On demand:** `asf rotate <connector>`, or `asf rotate --all-due`.

Approvals: a new class, `rotate_secret` (a sub-kind of touch_security). The operator decides per product whether scheduled or emergency rotation runs unattended.

Degradation: providers that can't mint credentials through an API get `mode: guided`, where ASF still does steps 2–5 (distribution, verification, audit, the revoke reminder). `mode: none` just tracks age and reminds.

Tests use fake providers:
- api rotation happy path; a consumer write fails → rollback, old credential not revoked
- guided mode never echoes the value
- a leak finding triggers rotation
- the audit line holds fingerprints only
- doctor shows age against max_age
