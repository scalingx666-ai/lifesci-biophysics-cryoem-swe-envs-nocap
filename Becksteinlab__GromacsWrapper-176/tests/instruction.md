# Issue

**Title:** expand test matrix

## Description

- fix #172
- test Gromacs 2019.1
- test Gromacs 2018.4 (upgraded from 2018.2)
- test Python 3.7

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `bc2ecf412a8f`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.