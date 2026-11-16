# Issue

**Title:** AbsoluteAlchemicalFactory options and CUDA deterministic forces

## Description

I still have to update the `whatsnew` file and the docs, but the implementation is ready for review.
- Add `AbsoluteAlchemicalFactory` options to YAML
- Fix #849: Set CUDA platform to use deterministic forces
- Fixes a bug that we missed when transitioning to Python3 where parallel MPI processes may pick a different counterion to alchemically modify due to the non-deterministic order of dictionaries.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `e68532bd3451`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.