# Issue

**Title:** Fix "unsupported operand" issue when accessing IOData.charge

## Description

This is the fix for the following type of error message:

```
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
  File "/home/maarten/anaconda3/envs/iodata/lib/python3.8/site-packages/iodata/iodata.py", line 274, in charge
    return self.atcorenums.sum() - self.nelec
TypeError: unsupported operand type(s) for -: 'float' and 'NoneType
```

This issue is hampering two PRs (#188 and #186), so I'm putting in a separate PR, which can be merged easily. 

(Related issues with nelec and spinpol are also fixed.)

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `cab161cd6e32`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.