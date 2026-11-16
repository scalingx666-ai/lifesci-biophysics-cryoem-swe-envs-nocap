# Issue

**Title:** [PATCH] Fixes issue #168 - charge assignment logic

## Description

Changed the charge exception handling to deal with gif files missing the charge information (e.g. from AF3 and Chai).
Tested and working fine using the provided cif file in issue #168

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `d2d0cf0c337e`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.