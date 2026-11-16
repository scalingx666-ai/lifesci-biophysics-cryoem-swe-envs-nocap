# Issue

**Title:** Finalize 0.4.0

## Description

- test 2.7, 3.5 (for the last time); add tests for 3.8 and 3.9
- update CHANGEs (deprecate 2.7 and 3.5)
- update docs
- contribute to #113

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `db503e3721c3`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.