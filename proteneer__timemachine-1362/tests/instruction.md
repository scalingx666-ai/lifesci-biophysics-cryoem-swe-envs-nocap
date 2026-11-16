# Issue

**Title:** Clean up atom mapping code a bit

## Description

* Test is no longer expected to take 30 minutes
* Don't do RMSD, since the square root is unnecessary
* No longer compute the bonds after atom mapping
* Uniquify without returning incorrect bonds
* Seems to improve wall clock time of tests by about 5%

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `a74310211dc7`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.