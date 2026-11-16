# Issue

**Title:** Addition of non-uniform priors.

## Description

This PR implements the ability to pass arbitrary scipy.stats objects as priors. This means that non-uniform priors can be passed to different FittingBase objects. 

However, to achieve this, I have removed the `bounds` keyword argument, which is an API change; hence, this will be a part of a 2.1.0 release to account for this.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `1276f816fa7c`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.