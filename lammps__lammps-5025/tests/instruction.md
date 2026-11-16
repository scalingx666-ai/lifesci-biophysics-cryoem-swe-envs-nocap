# Issue

**Title:** GPU package: avoid NaN energies with chipStar (HIP SPIR-V)

## Description

The `lib/gpu` block-wide energy/virial reduction assumes warp-synchronous execution, which the SPIR-V/OpenCL model does not guarantee — producing NaN energies/stresses on chipStar/Intel GPUs (forces are unaffected). Enable the existing `LAL_NO_BLOCK_REDUCE` flag for `HIP_ARCH=spirv` to accumulate energy/virial on the host instead.

Verified on Intel PVC (Aurora) and Intel UHD 770: GPU pair-style unit tests go from 28 NaN failures to 0 (297/297 pass).

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `91d4111a943e`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.