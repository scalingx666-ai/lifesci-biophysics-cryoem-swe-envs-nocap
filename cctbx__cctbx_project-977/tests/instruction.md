# Issue

**Title:** Fix miller array `repr` when `label_string()` returns `None`

## Description

I came across this whilst running  on data in space group $F d d 2$. Simplest solution: if `label_string()` returns `None` just wrap it in a `str()` call.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `fcc6b3b5d0bf`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.