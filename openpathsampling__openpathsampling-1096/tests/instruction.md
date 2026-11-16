# Issue

**Title:** make a missed string into a rawstring

## Description

This fixes a DeprecationWarning that popped up in the running tests after merging #787 
```
openpathsampling/engines/toy/pes.py:374
/home/sroet/github_files/openpathsampling/openpathsampling/engines/toy/pes.py:374: DeprecationWarning: invalid escape sequence \s
"""Simple double-well potential. Independent in each degree of freedom.
```

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `24143212881d`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.