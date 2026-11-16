# Issue

**Title:** fix: match cython types to C ones

## Description

In dataset.pxd

Currently dataset type is Py_ssize which is signed and of variable size, whereas dataset.h uses uint64_t. This may be leading to some alignment issues.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `5a335b6202d8`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.