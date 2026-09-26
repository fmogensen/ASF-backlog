# merge_skipped: a missing non-Actions required check counts as path-filtered

merge_skipped: path-filtered (e0f09258d) counts a missing required check as satisfied when no completed Actions run has a job with that name. A required check from outside Actions (a third-party app or a commit status) that has not reported yet would then pass as "not created by the workflow".

Expected: "missing" is satisfied only when the check name is known to come from an Actions workflow — for example the name matches a job in a workflow file on the head, or it appeared as an Actions check on an earlier run. Otherwise it waits.

Test: a required check absent from every run and never seen as an Actions job → waits.
