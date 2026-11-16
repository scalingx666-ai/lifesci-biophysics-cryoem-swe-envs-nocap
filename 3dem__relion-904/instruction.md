# Issue

**Title:** Fix for LLVM-based compiler

## Description

The existing "#pragma omp simd" in diff2_coarse function in CPU accelerated code is wrong since it requires private copy of partial array(diff2s) for SIMD vectorization. It is not supported by OpenMP standard. And this is causing incorrect result on LLVM-based compilers(Clang, Intel oneAPI, AMD AOCC, ARM clang, ...) while Intel and GCC compilers are okay.

And there is one fix for CMakeList.txt. It will fix Intel oneAPI compiler MKL support and this change is already applied "relion-devel" repo.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `fa923df15ad2`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.