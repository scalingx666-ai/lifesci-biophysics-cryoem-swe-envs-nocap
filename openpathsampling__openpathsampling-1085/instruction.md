# Issue

**Title:** OpenMMEngine: use masses from mdtraj topolgy directly

## Description

This avoids the error raised by the fact that virtual sites have no element associated with them in openMM by taking the mdtraj topology directly. From our discussion in #1084 I gathered that using the mdtraj masses directly would be an acceptable solution?

This avoids the situation in which we have to change the engine of the snapshots to be able to use the `RandomVelocities` modifier with virtual sites as in TIP4P. One should still pass a `subset_mask` in which the virtual sites are excluded and the OpenMMEngine as `engine` to be able to apply constraints to the `RandomVelocities` modifier on creation.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `ea605e8b8305`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.