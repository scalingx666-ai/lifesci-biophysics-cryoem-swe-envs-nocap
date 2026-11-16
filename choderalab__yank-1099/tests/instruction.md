# Issue

**Title:** Add YAML syntax tutorial and fix MC atoms

## Description

- Add a tutorial that cover the basics of the YAML syntax.
- Fix #1098: When redefining `MCDisplacement/RotationMove` in the `mcmc_moves` without specifying `atom_subset` caused the MC move to affect the whole system instead of only the ligand atoms.
- Add `net_charge` option to the `antechamber` pipeline for charged small molecules.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `e60bbf490016`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.