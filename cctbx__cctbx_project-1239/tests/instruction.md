# Issue

**Title:** mmtbx.ions.svm: remove unused hydroxyl distance tracking

## Description

`_is_favorable_halide_environment` in `mmtbx/ions/svm/utils.py` tracked the nearest SER/THR/TYR hydroxyl but never read the result. Its running minimum started as `None`, so a water within 3.5 Å of a THR OG1 raised

```
TypeError: '<' not supported between instances of 'float' and 'NoneType'
```

during halide filtering in ion placement (seen from phenix.refine with ion picking enabled). This removes the unused variables and branch; the function's return value is unchanged.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `86b07e447d60`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.