# Issue

**Title:** force level2 if bounding box

## Description

If a user is asking for a bounding box, then we need to force the server to use level 2 meshes, as the high level meshes will intersect with the bounding box and return a larger mesh than is desirable.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `5a87397e9ec0`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.