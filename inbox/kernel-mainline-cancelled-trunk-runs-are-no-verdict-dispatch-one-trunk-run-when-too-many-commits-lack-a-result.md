# Kernel mainline: cancelled trunk runs are no verdict; dispatch one trunk run when too many commits lack a result

mainline.py: a trunk tests run cancelled by a newer push is no verdict, not red/green; once more than kernel.mainline.max_pending commits have no finished required run, dispatch one workflow_dispatch trunk run on the newest head. Acceptance: tests for cancelled-by-newer and the dispatch threshold.

parent: F-0334 (ASF 0.3). Generic: no product named in code.

## Question
This reads as a defect. A Bug carries a signature — add signature: <the failing test or error line>, or paste that line into the body (an `Error:` line or a `file:line › test` line is read as one); or an ## Acceptance list if it is new work.
