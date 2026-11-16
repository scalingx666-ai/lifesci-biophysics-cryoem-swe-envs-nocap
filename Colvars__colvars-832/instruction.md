# Issue

**Title:** fix: save the gradients before calc_value in debug_gradients

## Description

This commit saves the gradients and fit gradients for all atom groups in a CVC before changing the atom positions in calc_value(), since some CVCs compute the gradients in calc_value() and therefore change the gradients buffers.

This commit also reverts 2bafda0346c0b6789f661ec211dea31675f7aabe since enable(f_ag_fit_gradients) is necessary for running possibly do_feature_side_effects that makes thing work.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `e45121f25c0a`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.