# Issue

**Title:** Fix testing against OpenMM RCs

## Description

Some change has caused the token for workflow dispatch (the mechanism we used to conditionally launch a workflow) to fail. However, in the meantime, GitHub has added a native approach for reusing workflows. So this is intended to switch to using that.

And we're probably going to have some things to change thanks to the OpenMM 8.0 release candidate....

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `53d90333421f`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.