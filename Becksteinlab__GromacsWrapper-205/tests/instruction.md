# Issue

**Title:** extend testing

## Description

- fix #202 (test multiple GROMACS versions 4.6.5, 2018, 2020)
- fix #203 (test on macOS)

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `407e9956869c`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.