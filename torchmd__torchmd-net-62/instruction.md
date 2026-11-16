# Issue

**Title:** Convert the unknown `load_model` parameters to warnings

## Description

This to enable to load the old check point files:

```python
model = load_model('old.ckpt', aggr='add')
```

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `61500f894992`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.