# Issue

**Title:** Fix CE-Symm bug with short repeats

## Description

This fixes rcsb/symmetry#118.

It switches the determination of the symmetry group and rotation axes to use the CESymm alignment directly. Before it was recomputed using QuatSymmDetector, which failed with short repeats.

A new CE-Symm version will be released after the next biojava release.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `53bdc0129646`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.