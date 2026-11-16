# Issue

**Title:** Reduce2 faster cliques

## Description

This selects a vertex cut that most evenly divides the remaining Movers into same-sized components, which greatly increases the speed of computations.

It also fixes a bug in the case where there is a time limit on the calculation and verbosity is 0.

It also prints the per-clique optimization time.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `35a799409b2a`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.