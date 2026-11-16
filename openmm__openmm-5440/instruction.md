# Issue

**Title:** Optimize CustomVolumeForce

## Description

CustomVolumeForce is a trivial force that is much slower than it has any need to be.  That's because it is implemented with CustomCPPForceImpl, which requires it to download positions to the host and upload forces to the device on every step, even though it doesn't use them.  I've changed it to subclass ForceImpl directly, making it much faster.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `ccc08b118eff`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.