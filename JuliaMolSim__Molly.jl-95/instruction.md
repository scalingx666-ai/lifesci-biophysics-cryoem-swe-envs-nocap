# Issue

**Title:** Speed up for softcore potentials and fixes to Mie

## Description

Initial implementation of `LennardJonesSoftCore` and `CoulombSoftCore` was quite slow (several microseconds for single force calculation). This PR add a speed up to make it useable (now the times are about 150ns per force calculation, 3x slower than non-softcore interactions but still way better than before).

Also  .

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `7d95bcc27217`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.