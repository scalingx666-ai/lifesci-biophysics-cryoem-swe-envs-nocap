# Issue

**Title:** Streamline the two exascale simulation kernels.

## Description

Debranch kernel now a) uses vec3 rather than Holton-vec4; b) marshals all mos-tics and phi-tics into one list of crystal orientations; c) aligns exact order of operations (*,/) with kokkosSpotsKernel.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `b959a753e132`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.