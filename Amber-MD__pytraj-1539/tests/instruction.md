# Issue

**Title:** FIX memory leak in stripping

## Description

Below code should run without increasing memory
```python
import pytraj as pt

orig_top = pt.load_topology('../../tests/data/tz2.parm7')


def test():
    for i in range(100000000):
        print(i)
        top = orig_top.copy()
        top.strip(":1")


test()
```

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `b633a6993a80`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.