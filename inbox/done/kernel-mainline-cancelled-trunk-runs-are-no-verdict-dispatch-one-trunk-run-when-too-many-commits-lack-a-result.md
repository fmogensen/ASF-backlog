→ F-0338

# Kernel mainline: cancelled trunk runs are no verdict; dispatch one trunk run when too many commits lack a result
type: feature
parent: E-0003

mainline.py: a trunk tests run cancelled by a newer push is no verdict, not red/green; once more than kernel.mainline.max_pending commits have no finished required run, dispatch one workflow_dispatch trunk run on the newest head. Acceptance: tests for cancelled-by-newer and the dispatch threshold.

