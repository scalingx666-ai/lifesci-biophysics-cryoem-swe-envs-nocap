# Issue

**Title:** ReaxFF GPU optimizations

## Description

This PR improves ReaxFF GPU performance through four main changes:

- tuning the kernel workgroup sizes when running on GPU.
- accumulating energy components on the device, reducing transfers and synchronization calls.
- fusing charge equilibration CSR construction, and tuning its reduction workgroup sizes.
- filtering bond candidates to reduce work in the bonded kernels.

In local benchmarks, these changes reduced simulation time per step by ~30% for the systems tested.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `f15492143b2d`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.