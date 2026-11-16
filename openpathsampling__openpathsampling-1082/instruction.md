# Issue

**Title:** unbreak API call to selector.probability_ratio

## Description

Of course merging #1075 let to a downstream CI of mine failing because it broke some API... :facepalm: 
it errors with:
```
  File "/builds/sroet/web_throwing/openpathsampling/openpathsampling/pathmover.py", line 841, in _build_sample
    new_snapshot=new_snapshot
TypeError: probability_ratio() got an unexpected keyword argument 'new_snapshot'
```

This catches the break and raises an appropriate warning instead of failing outright.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `4cb16cf78175`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.