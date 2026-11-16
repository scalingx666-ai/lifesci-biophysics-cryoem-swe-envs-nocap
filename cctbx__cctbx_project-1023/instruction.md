# Issue

**Title:** Dna vs rna ribbons

## Description

Properly configures ksdssp and includes it in all builds.

Handles combination of DNA and RNA within the same file in ribbons code by swapping the DNA but not the RNA.

Uses secondary-structure information found in the original PDB file if it is there rather than recomputing it.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `4dacb50b2514`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.