# Issue

**Title:** Support more flexible labels for trajectories

## Description

Allowed trajectory indices/labels to be non-consecutive integers.
Allowed trajectory indices/labels to not start from zero.
Allowed arbitrary labels for trajectories.

Behaviour for default remains unchanged.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `19b2285290ab`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.