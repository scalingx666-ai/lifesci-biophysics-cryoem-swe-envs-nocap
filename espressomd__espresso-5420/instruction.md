# Issue

**Title:** Fix packaging issues

## Description

Description of changes:
- fix packaging issues revealed by EasyBuild
- add missing feature guard in reaction methods involving ionic species

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `3ad68d4ed298`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.