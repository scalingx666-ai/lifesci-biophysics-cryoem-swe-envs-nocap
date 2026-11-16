# Issue

**Title:** Align  __contains__ with __getitem__ for IFP class

## Description

The following was failing unexpectedly despite the same key working for indexing:
```python
# works
fp.ifp[0]["LIG1.G", "ALA1.A"]

# doesn't work
("LIG1.G", "ALA1.A") in fp.ifp[0]
```

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `c28819fb74ea`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.