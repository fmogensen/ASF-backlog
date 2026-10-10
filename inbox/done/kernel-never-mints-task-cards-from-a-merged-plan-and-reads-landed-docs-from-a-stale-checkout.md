→ F-0343

# Kernel never mints Task cards from a merged plan, and reads landed docs from a stale checkout
parent: E-0003

## Description
The kernel tick never runs the plan -> Task minting step (asf/record/plan_tasks.py): only the old floor's run_record_fast called mint_plan_tasks. Of the Features planned on 10-09/10 none has a Task card, and a Feature whose plan PR merged is judged Done (kernel_state: done) because its own lane PR landed and nothing hangs under it. Also, the kernel reads landed specs from the product checkout's working tree (repo_dir), which can sit far behind origin/main (131 commits on 10-10), so landed documents are invisible.

## Fix
Each real tick fetches origin/<main> and reads landed specs and plans from that tree via git; Task cards are minted from every landed plan on the same tick (idempotent: a Feature with a Task child is left alone); a Feature whose plan landed but has no Task under it is not Done.
