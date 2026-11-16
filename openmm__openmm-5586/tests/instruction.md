# Issue

**Title:** Use a constant expression for torqueScale in hippoMultipoles.cc

## Description

RECIP((real) 0x100000000) expands to native_recip() on single precision OpenCL devices, which fails to compile on Apple's driver for Intel GPUs. The value is a compile-time constant, so write it as 1/(real) 0x100000000 like the other kernels do. .

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `463386e50e9c`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.