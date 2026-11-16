# Issue

**Title:** SimStore: Simplify OPS Storage constructor

## Description

Finally settling internally on the API: You can now set up an OPS storage with:

```python
storage = Storage(filename, mode)
```

No more of this `from_backend` stuff!

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `9495b543dee7`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.