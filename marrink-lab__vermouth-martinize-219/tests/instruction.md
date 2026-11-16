# Issue

**Title:** Fix typo in the definition of disulfide bridges

## Description

The link defining disulfide bridges in martini2 and elnedyn variants
were missing the function number in the constraint definition.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `bf858811b956`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.