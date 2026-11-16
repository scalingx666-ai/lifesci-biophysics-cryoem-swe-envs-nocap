# Issue

**Title:** Remove application code from rbfe module

## Description

Moves application code that is fairly specific to a use case (running a list of edges asynchronously) out of the `fe.rbfe` module to the example where it is used.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `dbb45e6f1064`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.