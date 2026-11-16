# Issue

**Title:** Version 1.2

## Description

The idea of this PR is to make all parameters differentiable up to their original values. In the current master branch we are precalculating the Lennar Jones tables from the original parameters before converting to a torch tensor meaning that we cannot differentiate all the way back.
Similarly for dihedral parameters we were duplicating the parameters for each dihedral instead of keeping unique parameters for each dihedral type before converting to tensor so we could not differentiate back to the original dihedral atomtype terms.

This PR fixes all this and allows us to optimize any parameters we wish.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `5f9b8011463a`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.