# ci_pool runs-on reader can't resolve fromJSON(format()) + event ternary (botseon #849)

botseon PR #849 switches 13 heavy jobs to `runs-on: ${{ fromJSON(format('["self-hosted","{0}"{1}]', vars.X || 'heavy', github.event_name == 'pull_request' && ',"class-pr-heavy"' || '')) }}`. asf/ci_pool.py RunsOn returns labels=None for any expression, so these jobs are "never judged". The doctor's unsatisfiable-runs-on check goes blind, and reconcile no longer sees that those labels are needed. Together with the known vars.* gap (card: reconcile ignores repo vars), `asf ci reconcile --apply` could strip labels that are in use.

Expected: the reader resolves `fromJSON(format(...))` over literal and `vars.*` arguments (read the repo's Actions variables) and `github.event_name == ...` ternaries. It yields one label set per event class (pull_request vs other), and a label used under any event counts as needed. Unresolvable expressions make reconcile refuse to remove anything.

Test: this exact expression → PR {self-hosted, heavy, class-pr-heavy}, other {self-hosted, heavy}.

## Question
Which Epic is this under? No open Epic shares a title word with it.
