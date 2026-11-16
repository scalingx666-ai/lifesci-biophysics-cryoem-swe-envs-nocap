# Issue

**Title:** Use robust mbar

## Description

- Solves an issue after updating to PyMBAR (possibly related to ) where MBAR doesn't converge and rebalancing results in a very poor lambda schedule

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `e4fb4303ad6d`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.