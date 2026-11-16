# Issue

**Title:** modernized CI workflow

## Description

- fix #253 
- use micromamba instead of mamba
- use CODECOV_TOKEN for codecov

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `05c3625f1d1a`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.