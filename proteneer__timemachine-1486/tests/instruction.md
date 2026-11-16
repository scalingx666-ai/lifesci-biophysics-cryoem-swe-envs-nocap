# Issue

**Title:** switch to use PyMBAR axis convention for u_kln

## Description

- Update the u_kln to use the PyMBAR convention (k is the state index, l is the parameter index)

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `b1a3108fcce0`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.