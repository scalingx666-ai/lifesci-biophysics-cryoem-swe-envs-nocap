# Issue

**Title:** Add Potential::execute_batch_sparse

## Description

Adds a "sparse" variant of the existing `Potential::execute_batch` function that can be used to evaluate a subset of elements of the dense (coords_batch_size, params_batch_size) matrix. Compared to `execute_batch`, `execute_batch_sparse` additionally accepts two arrays (of length `batch_size`) specifying the index of the coordinates and parameters, respectively, to use for each evaluation.

The present motivation is to enable more efficient computation of the $u_{kl}$ matrix used in HREX, since most of the energies in the dense matrix end up unused (we typically only need energies in the neighborhood of each replica).

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `847a1d967964`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.