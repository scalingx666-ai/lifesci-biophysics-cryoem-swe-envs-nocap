# Issue

**Title:** Add versioneer to track package version automatically

## Description

Fixes issue #803 

Also since I had to modify setup.py, I fixed the license and supported python versions, which fixes issue #788

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `c7896dc09a92`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.