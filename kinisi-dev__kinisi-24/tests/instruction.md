# Issue

**Title:** Bayesian bootstrap

## Description

Draft implementation of estimating var(MSD) using Bayesian bootstrap, using a sparsified Dirichlet prior to weight the observed data.
The number of independent (non-overlapping) observed displacements is now computed as an *effective* number of non-overlapping displacements — i.e., it includes fractional observations when Δt is not an integer factor of Δt_total.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `b0483477f53a`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.