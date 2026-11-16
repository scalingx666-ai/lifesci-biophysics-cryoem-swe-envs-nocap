# Issue

**Title:** Fix reference before assignment

## Description

Fix the bug described in #948 where a reference before assignment error was raised when setting up the calculation from an OpenMM systems.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `1aa436a9f909`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.