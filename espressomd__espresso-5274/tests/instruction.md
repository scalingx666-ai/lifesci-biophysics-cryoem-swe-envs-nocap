# Issue

**Title:** Fix build system regressions

## Description

Description of changes:
- add missing feature guards and conditional inclusion statements ()
- address CMake regressions revealed by openSUSE and MareNostrum users

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `9c458364bda0`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.