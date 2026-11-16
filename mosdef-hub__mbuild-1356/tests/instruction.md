# Issue

**Title:** Remove gpu-based numba kernels from `hard_sphere_random_walk`.

## Description

### PR Summary:

Initially, the idea was to have numba-cuda compatible versions of any method using regular (cpu) numba, but after doing some profiling recently, I think we should ditch this idea. Having N changing during each step makes for non-ideal conditions for writing to the GPU, even after trying to optimize the approach used, and therefore the performance improvement didn't occur until very large systems. Even then, the speed up wasn't significant enough to justify both 1) Carrying the additional code and maintenance complexity and 2) ensuring the user is only using `run_on_gpu` in the right conditions.

This PR removes all of the numba-cuda related logic/utils from develop.

### PR Checklist
------------
 - [ ] Includes appropriate unit test(s)
 - [ ] Appropriate docstring(s) are added/updated
 - [ ] Code is (approximately) PEP8 compliant
 - [ ] Issue(s) raised/addressed?

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `748e5554e4e9`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.