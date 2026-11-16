# Issue

**Title:** Optimize and simplify the calculation of dihedral gradients

## Description

This commit uses the equations (27i) ~ (27l) from
https://onlinelibrary.wiley.com/doi/10.1002/(SICI)1096-987X(19960715)17:9%3C1132::AID-JCC5%3E3.0.CO;2-T to simplify the calculation of dihedral gradients.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `cd91632912fd`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.