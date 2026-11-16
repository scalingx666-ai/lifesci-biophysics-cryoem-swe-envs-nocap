# Issue

**Title:** add newline after writing gro file

## Description

I added as default behavior that the gro file has a newline after the box line. This will make sure VMD can directly open the file. Note that we will however not be able to trajectory files. But I think we don't do trajectories anyways.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `b32ea367580b`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.