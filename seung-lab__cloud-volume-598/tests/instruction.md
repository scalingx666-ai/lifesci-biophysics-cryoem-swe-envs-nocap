# Issue

**Title:** perf: accelerate raw encoding speed for f order images w/o compression

## Description

Use a bulk encoding technique for transforming Fortran order image arrays into chunked fortran order byte objects. This uses more memory but is faster. We only use this new technique when the input image is appropriate and there is available RAM as measured by `psutil.virtual_memory().available`

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `37e00bec7c93`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.