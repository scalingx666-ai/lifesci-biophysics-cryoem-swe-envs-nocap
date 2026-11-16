# Issue

**Title:** adds ANI1x as dataset option

## Description

I took the self_energies from the ANI1 datasets. Given that ANI1x uses a different XC functional, the atomic 'self energies' should in principle be adjusted to reflect that.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `e193da7e3e17`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.