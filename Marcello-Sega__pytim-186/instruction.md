# Issue

**Title:** some utility functions becomes available as staticmethods

## Description

allows users to use `center_system` as a static method, without having to instantiate an interface, e.g.:

```
    pytim.PYTIM.center_system('planar',u.atoms,0)
```

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `4400591531b5`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.