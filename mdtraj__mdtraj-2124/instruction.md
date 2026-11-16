# Issue

**Title:** Fix PyPI upload GitHub Action Process

## Description

Use `pattern:` instead of `name:`

Bug from #1980. We might be able to manually dispatch this and it should hopefully push the right wheels up. If not we can just manually upload the wheels.

Last release it didn't trigger properly so it never got to this point...

Added an extra conditional so forks won't try to upload either.

EDIT: I manually uploaded the wheels and source.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `9bac88584841`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.