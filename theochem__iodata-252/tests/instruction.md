# Issue

**Title:** Improve treatment of occuptions numbers of restricted orbitals

## Description

.

The following remained as it was:

- Single `MolecularOrbitals` class for all kinds of orbitals.
- Restricted orbitals with integer occupation numbers are assumed to represent a restricted open- or closed-shell wavefunctions. E.g. occupation numbers `[2, 2, 1, 0, 0]` would still represent a doublet state.

The following has changed:

- Restricted orbitals with fractional occupation numbers are assumed to be spin-paired or closed-shell, unless specified otherwise. E.g. occupation numbers `[2, 2, 1.8. 0.2, 0]` are assumed to be spin-paired.

- It is possible to deviate from the above heuristics be specifying `mo.occs_aminusb`. Just keep in mind that most file formats cannot handle this case. They essentially rely on the above heuristics. Writing a wavefunction file will normally fail when `occs_aminusb` is set.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `7c221cbbcfdc`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.