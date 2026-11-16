# Issue

**Title:** Fix #917: Symmetry doesn't display with non-US locales

## Description

The core of this is the locale fix, which is necessary for CESymm/QuatSymm to function properly on non-US systems.

I've also included some commits for general cleanup and tests. If any of these are controversial I can revert them or move them to separate PRs.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `72eae71bb33d`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.