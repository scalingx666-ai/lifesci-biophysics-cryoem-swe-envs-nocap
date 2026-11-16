# Issue

**Title:** Improve detection of hard/mathematical boundaries

## Description

Implements the ability to auto-detect when user-defined boundaries are also "hard" mathematical boundaries.

See 

Related to #476 and  #710

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `65631d7e5e17`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.