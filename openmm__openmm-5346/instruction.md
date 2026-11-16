# Issue

**Title:** Fixed a GPU thread synchronization bug

## Description

.  This was a subtle threading issue that could cause incorrect results in RMSDForce, RGForce, and OrientationRestraintForce.  But usually it didn't.  It only seemed to occur when the GPU was trying to run multiple sets of kernels on different streams at the same time.  I really don't know why!

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `c94cdfb092cc`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.