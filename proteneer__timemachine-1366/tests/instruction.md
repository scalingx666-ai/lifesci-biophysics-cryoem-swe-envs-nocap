# Issue

**Title:** Assert that states differ only in their parameters

## Description

Improves the safety of an optimization we currently use in free energy computations where we only instantiate potentials for one end state and assume the other states differ only in their parameters.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `54789907ba1c`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.