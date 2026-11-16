# Issue

**Title:** Attempt at upgrading pytorch-lightning

## Description

We no longer seem to encounter problems with recent pytorch-lightning versions and jittable MessagePassing layers from pytorch-geometric. This PR upgrades the code base to be compatible with the changes made to the pytorch-lightning API from version 1.3.8 to 1.6.3.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `b1080b99671b`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.