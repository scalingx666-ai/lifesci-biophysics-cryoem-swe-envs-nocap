# Issue

**Title:** fixed center of mass formula

## Description

Old code returned with this error when attempting to calculate the center of mass for a molecule:

```
>>> benzene.center_of_mass
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
  File "chemlab/core/molecule.py", line 367, in center_of_mass
    return ((self.m_array * self.r_array)/self.m_array.sum()).sum(axis=0)
ValueError: operands could not be broadcast together with shapes (12,) (12,3)
```

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `3366c5143628`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.