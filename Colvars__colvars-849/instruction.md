# Issue

**Title:** opt: optimize the forces or gradients from q to the fitting group

## Description

This commit projects the forces acting on q, or the gradients of a CV with respect to q, onto the forces acting on the correlation matrix, or the gradients of a CV with respect to the correlation matrix at first. Then, the forces or the gradients on the fitting group are computed from the forces or the gradients on the correlation matrix.

By doing so, the number of multiplication operations in each iteration of the loop over fitting group atoms is reduced from 204 (64 * 3 + 4 * 3) to 9, and the number of addition/subtraction operations is reduced from 69 to 9.

The old routines like calc_derivative_wrt_group1 and calc_derivative_wrt_group2 are retained for Jacobian calculations.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `ebc285f78bf9`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.