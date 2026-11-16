# Issue

**Title:** Upgrade to Ubuntu 24.04 and Cuda 12.6.3

## Description

* Have to upgrade to at least Cuda 12.5.1 to get Ubuntu 24.04, upgraded as far as possible without making C++ changes.
* Previously I had gone all the way to 12.8.0 in #1488, but that required C++ changes that triggered a test failure. This only goes to 12.6.3. It also that there [may be issues in 12.8.0 tooling](https://forums.developer.nvidia.com/t/compute-sanitizer-kernel-name-exclude-option-not-filtering-kernels-in-cuda-12-8/323530). We should upgrade to Cuda 12.8.0 in another pass to get support for the RTX 50 series.
* Upgrades GCC from 9.4.0 to 13.2.0
* Upgrades NVCC from V12.4.131 to V12.6.85

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `20ef920961de`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.