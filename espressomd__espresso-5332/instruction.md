# Issue

**Title:** Add noise term in dpd pressure calculation

## Description

Fixes #

Description of changes:
- The random force (noise) term is added back to the DPD pressure.
-  With this, the Green-Kubo viscosity matches with Bindgen et al. (2021) paper and also with Groot-Warren theory.

<img width="1350" height="975" alt="image" src="https://github.com/user-attachments/assets/11d1a182-3d7a-4bf7-a4cd-eb4a6763d87a" />

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `8349cfff8d89`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.