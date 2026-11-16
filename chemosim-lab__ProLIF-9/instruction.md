# Issue

**Title:** Fix Issue #7: fp.to_dataframe is slow

## Description

Fixes issue #7 

- Updated the repr method of `ResidueId` so that it isn't confused with a string anymore
- Improved the speed of the `to_dataframe` function, from 8.5s to 150ms (TM3-GPCR PPI example)

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `550fd5a7b963`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.