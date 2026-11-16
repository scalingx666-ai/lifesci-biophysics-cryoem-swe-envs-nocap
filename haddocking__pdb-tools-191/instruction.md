# Issue

**Title:** [SKIP] Remove Python 3.9 and 3.10 from CI workflow.

## Description

3.9 has been unsupported for a while, 3.10 has been EOL since Oct 31.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `19d55fb7a13a`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.