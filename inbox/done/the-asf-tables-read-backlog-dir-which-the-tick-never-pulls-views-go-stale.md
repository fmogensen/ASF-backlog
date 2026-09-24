→ F-0122

# The /asf:* tables read backlog_dir, which the tick never pulls — views go stale

The /asf:* tables read the record at `backlog_dir`, the operator's checkout. The tick writes and pushes through its own clone at ~/.ASF/state/<product>/record, and never pulls `backlog_dir`. So the tables show a stale record unless the operator pulls by hand. The first customer install needed to learn "the two record copies" to understand what it saw. Found while writing the user guide.

Fix, pick one generically: the views read the tick's clone (read-only), or the tick fast-forwards `backlog_dir` when it's clean, and doctor says when it's dirty or diverged. The table header already prints "record: <path>"; it should also print the record's age ("index generated 3m ago").

Test: after a tick pushes, the next view shows the new state with no manual pull.
