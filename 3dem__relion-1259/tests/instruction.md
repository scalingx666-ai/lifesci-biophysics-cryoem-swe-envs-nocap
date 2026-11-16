# Issue

**Title:** SYCL optimization for cache friendly data structure

## Description

- Merge mdlReal and mdlImag to a single array with doubled size for cache efficient access
- Make required change and some cleanups for this mdlReal/mdlImag to mdlComplex in the required files in src/acc/ directory
- Change no_tex2D and no_tex3D to this mdlReal and mdlImag merging optimization (src/acc/sycl/sycl_kernels/sycl_utils.h)
- Remove unused complex2D and complex3D
- Change the location of group_barrier to remove unnecessary expensive barrier instruction

Compiling and running are checked for legacy, CPU, and CUDA acceleration code path.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `04cb97da8a46`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.