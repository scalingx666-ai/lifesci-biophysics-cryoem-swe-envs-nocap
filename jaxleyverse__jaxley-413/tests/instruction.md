# Issue

**Title:** Forward Euler and Cranck Nicolson solvers

## Description

This PR fixes forward Euler solvers and also implements a Cranck Nicolson solver.

Forward Euler raises for branched cells (or networks thereof).

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `19373f709208`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.