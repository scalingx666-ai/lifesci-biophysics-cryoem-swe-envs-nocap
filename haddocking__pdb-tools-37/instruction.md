# Issue

**Title:** Extended usage to allow selection of single residues, besides ranges

## Description

Addresses #34 

Allows for multiple ranges, single residue selections, or both:
`pdb_selres -1:10,20:30 random.pdb`
`pdb_selres -1,10,22,56 random.pdb`

EDIT:
In addition, the code now allows for a proper printing of n-th residues (when using the step option), by using the actual resid list instead of a sequential range.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `48323bc0c89f`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.