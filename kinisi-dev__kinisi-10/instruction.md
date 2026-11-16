# Issue

**Title:** Changing the approach to variance estimation

## Description

In this PR, I have adapted the approach that is taken to find the variance from covariance matrix and mean. Now instead of using likelihood sampling, we produce a range of random samples from the Gaussian process and apply GLS (in a vectorised fashion) to all of these samples. This has involved the introduction of the `rtol` parameter, which is associated with the tolerance of the matrix inversion for the construction of the Gaussian process. Additionally, the tests have been updated and the documentation (including methodology) modified to reflect this change.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `992e1715d3e4`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.