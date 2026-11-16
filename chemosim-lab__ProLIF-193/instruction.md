# Issue

**Title:** Fix build test

## Description

Some test files are not included in the published tar.gz build for some reason, but they are in the CI build...?

TODO:
- fix build pipeline running twice on push

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `507280bda3f4`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.