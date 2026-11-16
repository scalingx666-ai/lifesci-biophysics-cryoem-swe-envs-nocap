# Issue

**Title:** `zstandard` import bandaid

## Description

The plan agreed to on 9/10 is

- [x] Import `zstandard` at the top of `openeye_wrapper.py`
- [x] Make a custom off-label build without `zstandard` constraint
  - [x] Look for `noarch/openff-toolkit-0.16.4-pyhc2fc6b4_3.conda`
- [x] Test locally with custom build (see below)

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `58b06880fc7c`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.