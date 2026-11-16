# Issue

**Title:** EntropyFingerprint: neighbors farther than some 40 sigma count

## Description

The Gaussian of each neighbor in the pair entropy was built up by a recurrence starting at r = σ. Its first value, exp(−(r/σ − 1)²/2), underflows to zero for r/σ above about 39.6, and the neighbor then added nothing. With the default σ = 0.2 every neighbor beyond 7.9 Å was lost; with σ = 0.1, beyond 4 Å. Below the threshold the results were right.

The recurrence now starts at the grid point nearest the neighbor and runs outward both ways. It stops where the Gaussian vanishes.

Evidence:
- New tests compare with the NumPy reference for cutoffs of 60σ and 45σ, with global and local density, and for two atoms 40σ apart (−0.50 before, now equal to the reference). They fail on master.
- Against LAMMPS `compute entropy/atom` on fcc Al, with noise of 0, 0.05 and 0.12 Å and σ of 0.1, 0.15 and 0.2: within 0.7 % in every case. For σ = 0.1 and a cutoff of 6 Å the means are −24.72 and −24.87; before the fix MDAPY gave −30.5.
- The page now says 0.7 % rather than 0.5 %.

Found by a review.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `e2e657e92ae4`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.