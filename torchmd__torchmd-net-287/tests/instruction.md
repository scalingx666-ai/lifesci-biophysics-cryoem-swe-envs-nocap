# Issue

**Title:** Use switching function for Coulomb prior

## Description

Originally I scaled the Coulomb prior by $\text{erfc}(\alpha r)$ to reduce its effect at short distances.  It turned out this didn't work well because it was still too short ranged.  Even at distances of only 1-2 A there was already a large component of Coulomb energy.  I changed it to a cosine switching function which works much better.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `5a4fca740d10`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.