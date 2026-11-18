# Issue

**Title:** Reshape empty arrays to allow concatenation of empty skeletons

## Description

Running `Skeleton.simple_merge(skels)` fails if any of the skeletons are empty, since the length of dimension 1 is set to 0. This change simply reshapes the empty arrays to the correct shape to correct for this.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `2fa245245ffd`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.