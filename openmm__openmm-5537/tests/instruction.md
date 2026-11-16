# Issue

**Title:** Fix issue #5536: Add virtual destructor to ComputeForceInfo

## Description

This PR  by adding a virtual destructor to `ComputeForceInfo`. Contexts delete force infos through the base class, so subclass members such as the per-particle and per-exception vectors in the `NonbondedForce` `ForceInfo` were never freed. This was debugged using `heaptrack`.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `308e446242e3`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.