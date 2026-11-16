# Issue

**Title:** feat: default login URL scheme http://

## Description

Can now login without specifying `http(s)://` in the URL, e.g.,

```cli
python -m cryosparc.tools login --url localhost:40000
```

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `fb458c707b3c`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.