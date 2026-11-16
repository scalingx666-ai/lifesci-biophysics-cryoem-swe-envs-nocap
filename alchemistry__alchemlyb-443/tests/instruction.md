# Issue

**Title:** prepare for release 2.5.0

## Description

- separate build and deploy steps
- use trusted publishing in deploy env
- updated license string to SPDX format
- updated README
- removed deprecated lower case estimator names in convergence
- prepare CHANGES for release

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `e4a56dfbaad2`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.