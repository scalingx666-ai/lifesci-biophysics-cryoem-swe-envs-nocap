# Issue

**Title:** Add ability to run `HardSphereRandomWalk` on GPUs using numba's `cuda.jit` methods.

## Description

### PR Summary:
This PR creates a new utils file (`path_utils_gpu.py`) where some of the numba jit functions (in `path_utils.py`) were converted to work with numba's cuda jit methods. From some initial scaling tests there are definitely improvements at large system sizes. I still need to test out the gpu versions of some of the Bias methods.

### PR Checklist
------------
 - [ ] Includes appropriate unit test(s)
 - [ ] Appropriate docstring(s) are added/updated
 - [ ] Code is (approximately) PEP8 compliant
 - [ ] Issue(s) raised/addressed?

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `67d3317d69b8`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.