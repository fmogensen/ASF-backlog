# Stories-first: validated-config read and a review of the last fix commit of PR #1670
parent: F-0334

Type: Bug

T-159853 (PR #1670, Stories-first ordering and the pace cap) was landed by the console after 4 fix rounds at a green head. Its last fix commit was not reviewed, and one review finding was left out on purpose. Stories-first is off by default, and the live kernel is a pinned earlier build, so nothing changed in production. Close these before the key is turned on or this code is installed as the kernel.

Open:
- asf/kernel/ports.py:3153 (and :3165): _stories_first and _launch_per_tick read the raw kernel.launch block and ignore the validated one, so a default set in settings.SPEC never reaches Config. Give both readers the validated block and prefer it when the key is present; keep the raw read as fallback; add the test beside LaunchKnobs in tests/kernel/test_finish_first.py.
- Review the last fix commit of PR #1670 (the _stories_of change in decide.py, STORIES_FIRST_DEFAULT = False, the ci_free gate in ports.py) as a normal review and fix what it finds.

Acceptance:
- Each fix has a test that fails before and passes after.
- kernel.launch.stories_first set in a product yaml reaches Config through the validated block.
