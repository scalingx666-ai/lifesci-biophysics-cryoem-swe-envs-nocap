# Issue

**Title:** Fix typo in ndof attribute

## Description

In `WaterBox` and `WaterCluster`, the `ndof` attribute was computed incorrectly when `constrained=True` -- only 3 d.o.f. were subtracted from the system, regardless of the number of waters in the system.
```python
>>> WaterBox(constrained=False).ndof
4509
>>> WaterBox(constrained=True).ndof
4506
```

Fix: subtract 3 d.o.f. per water.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `87ebee6a2bf3`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.