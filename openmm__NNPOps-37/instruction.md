# Issue

**Title:** Fix the execution of ANISymmetryFunctions on a non-default CUDA stream

## Description

`ANISymmetryFunctions` give incorrect result if executed on a non-default stream.
- [x] Implement `CudaANISymmetryFunctions::setStream` and `CudaANISymmetryFunctions::getStream`
- [x] Set the CUDA stream in `NNPOps::ANISymmetryFunctions::Holder`
- [x] Add tests
- [x] Document

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `6939536c9958`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.