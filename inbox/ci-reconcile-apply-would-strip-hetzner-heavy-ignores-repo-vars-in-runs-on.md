# ci reconcile --apply would strip hetzner-heavy: ignores repo vars in runs-on

Found 2026-09-26: `asf ci reconcile --apply` for botseon would remove the `hetzner-heavy` and `contabo-heavy` labels from the heavy runners. Its ci.yml parser takes the default `heavy` from `${{ vars.CI_REQUIRED_LABEL || 'heavy' }}` and ignores the repo variable CI_REQUIRED_LABEL=hetzner-heavy, which main's required jobs run on. Applying it would leave main's required jobs with no runner.

Expected: reconcile resolves `vars.*` in runs-on from the repo's actual Actions variables (gh api repos/<r>/actions/variables) before deciding which labels are in use. It never removes a label that any resolved runs-on uses. Until then, --apply refuses to remove non-class labels.

Also: contabo runners h1..h3 carry a `hetzner` label (cosmetic, but misleading).

Test: runs-on with vars.X set to a label → that label is kept.

## Question
Which Epic is this under? No open Epic shares a title word with it.
