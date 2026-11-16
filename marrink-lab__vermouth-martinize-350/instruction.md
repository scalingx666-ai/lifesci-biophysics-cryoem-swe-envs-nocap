# Issue

**Title:** Make cter and nter residue specifications only match proteins

## Description

This should make the whole martinize2 experience a bit smoother, since this makes the mutation/modification annotator only affect protein residues for `cter` and `nter` residue specifications.

Most changes are in the tests: renamed residue A to GLY, B to PHE and C to CYS so they're actually protein residues. Also added a non-protein residue to be sure.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `558a3966d679`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.