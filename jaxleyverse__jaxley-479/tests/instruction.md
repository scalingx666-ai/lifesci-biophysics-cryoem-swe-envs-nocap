# Issue

**Title:** Speed up tests (swc, plotting)

## Description

Main contributions:
- swc tests now significantly faster (change to sparse solver)
- plotting tests a lot faster
  - exposed 'resolution' of mesh generation (no proper exposition to user (yet)) 
  - splitted plot volume test for more granular analysis

minor contributions:
- added test fixtures for compartment, cell, morph_cell, branch and network -> not sure how much it helps / improves readability here. Could also revert these changes. (might be a first step for #449)

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `981ab6412e88`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.