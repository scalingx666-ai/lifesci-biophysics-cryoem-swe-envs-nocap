# Issue

**Title:** Replace copies with swap in `simulate!`

## Description

In a couple places we would do

```
a .= b
b .= a
```

instead of `a,b = b,a`. This PR fixes all those places and removes a copy from every single iteration of VV. Quick benchmark showed like 5% performance improvement which should apply to all simulations that use VV.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `ac668a98c73e`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.