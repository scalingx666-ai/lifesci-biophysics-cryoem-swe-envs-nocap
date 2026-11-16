# Issue

**Title:** Avoid ASA calculation for redundant NCS interfaces

## Description

Making ASA calculation of interfaces more streamlined, thanks to #732

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `92b548cf8229`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.