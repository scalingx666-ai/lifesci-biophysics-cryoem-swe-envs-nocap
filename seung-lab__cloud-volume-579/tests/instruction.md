# Issue

**Title:** Make vol.transfer_to() compatible with fill_missing

## Description

When I ran `create_transfer_tasks(fill_missing=True)` tasks, they still raised `EmptyFileException: ... were empty or had IO errors` in the `src_cv.image.transfer_to()` (line 420, tasks/image/image.py) . Thus, I added extra handling to "empty but no IO error" files in the `vol.transfer_to()` (precomputed/image/\_\_init\_\_.py).

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `1f9822285691`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.