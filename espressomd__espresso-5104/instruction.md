# Issue

**Title:** Improve MPI and CUDA portability

## Description

Description of changes:
- address compiler diagnostics from GCC 14.1 and nvcc 12.9
- modernize and simplify functions that collect GPU metadata
- remove hardcoded hostname and device name string lengths
- handle `OMP_NUM_THREADS` default value at the C++ level

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `8aa60cecd56c`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.