# Issue

**Title:** Update pressure convergence

## Description

Based on suggestion from @pmrv use only the last block to calculate average pressure.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `29fd253f034e`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.