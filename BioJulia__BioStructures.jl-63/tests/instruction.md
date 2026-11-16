# Issue

**Title:** Fix computing `Transformation` based on pairwise alignment

## Description

Thanks for maintaining this package, saves me a lot of work!

I have found a subtle bug in the function that computes a `Transformation` based on a pairwise alignment. In it, we want to collect all atoms belonging to residues that were (mis)matched in the alignment. To ensure that the number of atoms that are later used for the Kabsch algorithm are equal, we have to check for every residue if their atom counts match, even if the residues produced a mismatch in the alignment. However, the previous code only checked if the so-far collected atoms have the same amount (the invariant we want to maintain) where it _should_ check if the atoms about to be added have the same amount.

I adapted the code and added a short explanatory comment. Let me know if anything else should be added!

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `7e0f427b9753`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.