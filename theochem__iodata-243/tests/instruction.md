# Issue

**Title:** Adding additional bond type support when reading MOL2 files

## Description

I noticed that the mol2 format reader only supports bond types 1, 2, 3 and am
However, there are a few more defined in the original specification. This PR will add the support for these to the code.

Similar to assigning "am" to 4 it will also just assign the other bond types to increasing integers.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `08fa230e48a3`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.