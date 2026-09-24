# Config reader: a YAML flow-style map loads silently as a string — parse it or reject it clearly

ASF's config reader doesn't parse YAML inline maps (flow style `key: {a: 1, b: 2}`). Such a value loads as a string. With `quota_guards: {…}`, as the shipped example had it, the quota guard then crashed. The example is fixed, but any operator who writes flow style hits it again. Found while writing the user guide.

Fix: the reader either parses flow maps and flow lists, or rejects them with a clear error naming the key and line ("flow-style map not supported: write it as a block"). Never load them silently as a string. doctor reports it.

Test: a flow map is parsed or cleanly rejected, and the quota guard never sees a string.

## Question
Which Epic is this under? No open Epic shares a title word with it.
