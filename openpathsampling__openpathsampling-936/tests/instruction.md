# Issue

**Title:** Fix collections.abc DeprecationWarning in sample.py

## Description

Going from 5570cd31 to ace9d7e7 and running the tests surfaced another 
```
/home/sroet/github_files/openpathsampling/openpathsampling/sample.py:10: DeprecationWarning: Using or importing the ABCs from 'collections' instead of from 'collections.abc' is deprecated since Python 3.3,and in 3.9 it will stop working
    from collections import Counter, Mapping
```

This solves that one (`Counter` still has to be imported from base `collections`)

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `ace9d7e7511b`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.