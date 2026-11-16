# Issue

**Title:** Move CLI imports into functions for speed

## Description

In order to prevent the CLI from taking long times due to importing everything, especially when the user mis-types a command for docopt to handle, import statements for the CLI functions have been moved to the actual functions instead of the module header

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `4fd12d0e4c58`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.