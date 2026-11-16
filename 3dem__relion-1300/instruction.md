# Issue

**Title:** bugfix: reconstruct particle contrast inversion

## Description

During the subtomogram extraction initial step of STA particle reconstruction, contrast inversion of the entire particle stack should happen only one time per particle, not per every frame (this will cause multiple redundant inversions, and for an even # of frames, will not even do the contrast inversion since it all cancels out).

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `c2d0abaf1c6e`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.