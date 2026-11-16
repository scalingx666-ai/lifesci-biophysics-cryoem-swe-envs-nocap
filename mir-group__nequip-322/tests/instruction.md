# Issue

**Title:** Fix optimizer defaults

## Description

Adam -> AMSGrad based on findings.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `55bfc1f2aaca`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.