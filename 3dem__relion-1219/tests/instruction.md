# Issue

**Title:** Ver5.0 HIP Fixes

## Description

Minor updates to: 
* CMake config to correctly identify the HIP path
* `src/acc/hip/` files update paths of HIP lib headers (`'hipfft.h`, `hiprand.h`, `hipcub`)
These updates are required to ensure compatibility with newer ROCm > 5.7 and support on latest AMD Instinct GPUs.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `140fb8f55180`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.