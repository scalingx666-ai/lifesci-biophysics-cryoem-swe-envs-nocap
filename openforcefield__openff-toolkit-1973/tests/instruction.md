# Issue

**Title:** Run CI on `macos-latest`, not `macos-12`

## Description

Example impact: https://github.com/openforcefield/openff-toolkit/actions/runs/12167392775/job/33935854048?pr=1972


```
$ grep -r macos-1 . | wc -l
       0

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `ead8542045c1`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.