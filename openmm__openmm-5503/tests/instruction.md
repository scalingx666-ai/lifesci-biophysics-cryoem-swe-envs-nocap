# Issue

**Title:** Support non-AMD HIP platforms in SYNC_WARPS and warpShuffle

## Description

Add support for non-AMD HIP implementations

The following is AI-generated (verified against the code and by running the tests):

. When not compiling for an AMD GPU, SYNC_WARPS uses `__syncwarp()` and warpShuffle() uses `__shfl(word, src_lane)` instead of the AMD builtins. `__shfl` wraps the source lane modulo the warp size like ds_bpermute does, so the threadIdx.x+1 lane that warpRotateLeft passes still works.

The gate is `__AMDGCN__`, which clang predefines for every AMDGPU device compile, rather than `__HIP_PLATFORM_AMD__`: hipRTC from ROCm 5.7 does not define `__HIP_PLATFORM_AMD__` in runtime-compiled source, and testing it there would select `__syncwarp`, which ROCm 5.7 does not have.

AMD code is unchanged: for a test kernel using warpRotateLeft and SYNC_WARPS, the device IR from ROCm 5.7 clang (gfx90a, and gfx1100 with AMD_RDNA) and the gfx90a code object from ROCm 5.7 hipRTC are identical before and after this change.

This PR was generated using AI

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `e1c39e7844b2`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.