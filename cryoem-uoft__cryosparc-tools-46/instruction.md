# Issue

**Title:** Fix: ensure python object references decremented

## Description

When dataset is deleted, ensures internal object references can get garbage collected. Ported from `develop`

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `15e62a01cecc`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.