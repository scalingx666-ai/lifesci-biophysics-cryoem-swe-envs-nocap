# Issue

**Title:** Avoid setting global numpy PRNG seed

## Description

Since #1294, the default HREX implementation no longer relies on numpy global PRNG state. This PR modifies the signature of `attempt_neighbor_swaps_fast` to accept an explicit seed rather than generating one using `numpy.random` and removes an associated warning.

Minor unrelated change: explicitly declare integer dtypes for proposed and accepted count arrays

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `06f338d41d34`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.