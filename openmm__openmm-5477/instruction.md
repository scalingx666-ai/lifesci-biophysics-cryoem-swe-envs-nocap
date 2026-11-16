# Issue

**Title:** Fix handling of dimensionless units

## Description

.  Multiplying a number by a dimensionless unit should result in a scaled number instead of a dimensionless Quantity:

```pycon
>>> 3*(nanometer/angstrom)
30.0
```

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `5ee2cba0ccbf`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.