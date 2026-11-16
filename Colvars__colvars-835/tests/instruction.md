# Issue

**Title:** Initialize quaternions to zero, rotations to unity

## Description

to support the unlikely case of combinations of quaternion cvcs.

Rotation objects are now initialized to unity, which is more consistent.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `0b0978af19d0`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.