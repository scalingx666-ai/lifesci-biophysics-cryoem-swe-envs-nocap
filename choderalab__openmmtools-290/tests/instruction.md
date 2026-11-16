# Issue

**Title:** Generalize restrain atoms to non-protein receptors.

## Description

- Fix #285 : Allow passing a list of atom indices to `restrain_atoms()`.
- `forcefactories.restrain_atoms()` now computes the center of geometry of the atoms given by  `Context.getMolecules()` that match the restrained atoms.
- The center of geometry is moved to the origin only if the system is in NPT ensemble.
- An error is raised if the restrained atoms belong to multiple molecules.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `7125729ef81b`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.