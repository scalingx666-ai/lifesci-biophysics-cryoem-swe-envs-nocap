# Issue

**Title:** move header writing in gmx files to a system level meta attribute

## Description

instead of collecting the header for gmx files as a list, move it to a system.meta["header"] list that can be more dynamically added to

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `df2a554eb2fe`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.