# A pre-commit hook pointing at the /x/asf test fixture path broke review-b-0111's commit

review-b-0111 failed with 'hook refused': the pre-commit hook exec'd a hardcoded /x/asf, the fixture path from tests/test_check_staged.py. By 17:15 the live hooks were clean again, so the failure was transient. Still, a fixture hook ended up where a worker's commit could run it. Tests that install hooks must only write into their own git-init'd temp repo, and ensure_git_hooks must refuse a which() path that doesn't exist.
