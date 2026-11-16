# Issue

**Title:** get_cores raises exception when total nodes visits exceed max_visits and no cores are generated

## Description

* `max_visits` now means the total number of nodes visited rather than the max number of visits per threshold (which I _believe_ to be edges). This enables users to enforce more consistent wall clock times
* Raise NoMappingError exception rather than assertion to make the errors raised more uniform

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `3a174e63d32f`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.