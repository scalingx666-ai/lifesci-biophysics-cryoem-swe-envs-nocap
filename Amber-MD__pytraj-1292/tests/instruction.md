# Issue

**Title:** adding _format_convert

## Description

a simple method that use `CpptrajState` for converting data format

``` python
pt.io._format_convert('my.dx', 'my.ccp4')
```

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `8f636fc57669`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.