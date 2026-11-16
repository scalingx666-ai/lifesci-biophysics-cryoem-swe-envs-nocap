# Issue

**Title:** Fix: better test for MTS eABF

## Description

compare time step factor to proxy instead of 1
to enable again eABF with globalMasterFrequency

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `ea1c3b8d6d1c`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.