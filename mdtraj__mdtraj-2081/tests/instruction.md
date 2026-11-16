# Issue

**Title:** update dependabot weekly and pre-commit monthly

## Description

As per  we're updating pre-commit monthly. Also doing dependabot updates weekly, since we really don't need every minuscule point update.

Doing it less frequently allow updates to stack.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `c8a7b1b92ffc`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.