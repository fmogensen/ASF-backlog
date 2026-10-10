# Factory GitHub identity: GitHub App installation token instead of the operator's personal token

The kernel and all sessions share the operator's personal GitHub token (5000 REST/h); it was exhausted at 07:25Z on 2026-10-10 and the factory ran blind until the reset. Support a GitHub App installation token for the factory (product config: github.app_id + key path, operator-created), refreshed by the host, used by the kernel ports and passed to sessions; fall back to gh auth when unset.
Acceptance: with an app configured, kernel and session gh calls carry the installation token (test with fakes); without it, behaviour is unchanged.
