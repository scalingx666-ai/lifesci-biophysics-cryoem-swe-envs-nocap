# Issue

**Title:** Capture of floating point exception

## Description

With warnings enabled, Fortran gives an error if the argument of an exp is below ~-500, when getting close to 0. This is triggered in `h exp(−(x-a₁)²/(2a₂²)−(y-b₁)²/(2b₂²))-(z-c)=0`

Captured the exception by setting `h exp(−(x-a₁)²/(2a₂²)−(y-b₁)²/(2b₂²)) = 0` if `−(x-a₁)²/(2a₂²)−(y-b₁)²/(2b₂²) <= -50`

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `2360a0cdf45f`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.