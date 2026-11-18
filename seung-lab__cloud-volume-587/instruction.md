# Issue

**Title:** feat+perf: use precomputed ROIs to improve performance on empty regions

## Description

ROI format:

Added as key "rois" to the mip 0 scale in the info file.

List of lists of 6 integers [ xstart, ystart, zstart, xend, yend, zend ] specified in voxels at that scale.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `9b11d8e6f1ae`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.