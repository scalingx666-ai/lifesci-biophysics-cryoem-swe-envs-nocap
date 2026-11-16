# Issue

**Title:** Cleanups in iodata.basis

## Description

There are useful elements from #175 that don't need further discussion. I'll merge this after tests have passed. For other points from #175 and points mentioned in #146, I will open separate issues to get more bite-sized chunks and potentially some discussion before making (smaller) PRs.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `7d468483a504`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.