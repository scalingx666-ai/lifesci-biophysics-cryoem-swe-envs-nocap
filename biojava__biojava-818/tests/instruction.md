# Issue

**Title:** Fix CE-Symm errors

## Description

This provides shallow fixes for #816 and #817. By 'shallow' I mean that it fixes exceptions which were being thrown, but that there may be deeper issues. Neither bug is seems particularly serious, so I'd suggest merging this now and look for deeper issues later.

I have a test case for #817. It is kind of slow (runs CE-Symm) and needs structure-gui, so I've chosen to include it in the rcsb/symmetry project rather than in BioJava.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `a0f466dc92fe`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.