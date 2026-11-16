# Issue

**Title:** Add option to enable REST in single-topology transformations with HREX

## Description

Adds an option to enable REST-like enhancement of intermediate states in single topology transformations via scaling of certain interactions:

* ligand proper torsions (i, j, k, l) where (j, k) is a rotatable bond
* ligand proper torsions (i, j, k, l) where (j, k) is an aliphatic ring bond
* ligand-ligand coulomb and LJ
* ligand-environment coulomb and LJ

The effective temperature of these interactions in intermediate states is controlled by a configurable symmetric interpolation function (linear, quadratic, and exponential functional forms are predefined):
<img height=200 src=https://github.com/user-attachments/assets/f63a5ca1-aa24-44a0-a0cf-5aedf46a9773>

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `551b4b0dab8e`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.