# Issue

**Title:** Fix fully connected boundaries

## Description

Under some hard to reproduce conditoins, we non-bonded loop misses pairs for a system with regular decomposition and Lees-Edwards shear boundaries.
Likely cause is an off-by-one error in the
at_boundary() helper when constructing cell neighborships for the regular decompositoin with fully_connected_boundaries.

This Pr has a tentatie fix, but no unit test demonstrating the bug, yet.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `58f831cdeeec`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.