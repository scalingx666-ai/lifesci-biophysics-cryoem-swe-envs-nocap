# Issue

**Title:** Support atom-wise and molecule-wise priors

## Description

Implement the support of atom-wise (e.g. atomic reference energies) and molecule-wise (e.g. electrostatic term) priors.

- [x] Refactor the priors
- [x] Implement pre-reduce and post-reduce methods

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `35cb19acd354`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.