# Issue

**Title:** fix(encode_jpeg): correct 3D->2D shape for Neuroglancer

## Description

Array shape mixup in between the transposing / axis swapping / reshaping transformations.
Happened during the switch to simple_jpeg (#392)

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `545aba2543e6`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.