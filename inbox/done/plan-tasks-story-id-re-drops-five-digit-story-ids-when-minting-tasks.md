→ B-121289

# plan_tasks STORY_ID_RE drops five-digit Story ids when minting Tasks
signature: plan_tasks STORY_ID_RE drops five-digit ids S2

Bug: asf/record/plan_tasks.py:34 STORY_ID_RE is r'\bS-\d{4}\b' (exactly four digits), so no five-digit Story id on a plan's stories: line reaches the minted Task card. 790 of the stories: lines under docs/plans/ use five-digit ids (vs 25 four-digit). The Task mints with no stories field and has_task_child stops the Feature being re-read, so the loss is silent and permanent. Found by plan F-0330 (2026-10-10).

Fix: STORY_ID_RE accepts \d{4,}.
Acceptance: tests/test_plan_tasks.py covers stories_of with a four-digit and a five-digit S-id, both minted onto the Task card.
