# Issue

**Title:** switch CI from Travis to GitHub actions

## Description

- close #200 
- Limitation: only installs latest GROMACS for test and only Linux

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `017b10a80165`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.