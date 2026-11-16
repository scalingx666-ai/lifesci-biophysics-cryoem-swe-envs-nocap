# Issue

**Title:** fix LangevinSplitting

## Description

Fixes a bug in `Langevin Splitting`. For certain splittings (such as "BAOAB"), the first "B" step will not recompute forces for the sake of efficiency. Before this PR, the forces were initialized via a`similar` call, which may result in `NaN`s propagating during the integration steps.

The PR fixes this by precomputing the forces once before the integration takes place.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `7b1f9721aaa1`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.