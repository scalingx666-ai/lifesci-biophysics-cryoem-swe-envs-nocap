# Issue

**Title:** Fix leaked variable causing spurious template constraint assignment

## Description

Goals
- [x] Fix #5234 
- [x] Add test

Status
- [x] Ready for review

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `b55e60882dff`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.