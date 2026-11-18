# Issue

**Title:** Allow custom `jaxley` solvers to have different nseg per branch

## Description

Following #418, this will also allow different nseg per branch for the jaxley.thomas solver. This works by masking the tridiagonal system in every branch.

In addition, it also uses more invasively `.comp_edges` to build the conductances of the custom solver more easily. Before this PR, `.comp_edges` had only been used for the `jax.sparse` solver.

Replaces #425 which was closed due to difficult merge conflicts.

In addition, this contains a bugfix and .

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `26491413ecfb`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.