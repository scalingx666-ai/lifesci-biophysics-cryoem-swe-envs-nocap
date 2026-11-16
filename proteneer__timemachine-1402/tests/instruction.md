# Issue

**Title:** Fixes Support for Disabling Optimizations

## Description

This PR resolves the inability to build and test the repo without `-O3`. Due to having multiple definitions of `allocate` in the `timemachine` namespace, the `allocate` defined in `DeviceBuffer` was being used to allocate pinned memory when `-O3` was not included, but worked as expected with `-O3`.

The other changes in the PR:
* Use `static` and unnamed namespaces to isolate functions and structs inside of impl files. Should be part of our best practice going forward.
* If env var `DEBUG_BUILD` is set, disable `-O3` and add debugging information. Useful to catch issues like this in the future. 
* Adds `-O3` to gcc. Looks like this makes some tests 15% faster :exploding_head: 
* Prints both the GCC/NVCC compiler flags, just to be consistent and to make sure things are work as expected.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `49a9da70709b`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.