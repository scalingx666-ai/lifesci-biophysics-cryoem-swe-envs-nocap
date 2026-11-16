# Issue

**Title:** Split of propers and impropers from torsions, add ST symmetry test.

## Description

This PR:

1. Splits improper and proper torsions from a single torsion array - this will allow us to fine-tune the alignment and the interpolation logic in a separate PR. This can change expected results for single topology simulations: since we no longer allow an proper torsion to be accidentally aligned and interpolated into an improper torsion - they are now strictly separated. For all other types of simulations, (eg. vanilla MD, AHFE, water sampling etc.) results should be bitwise identical.
2. Removes `Optional[]` tags from the `[Vacuum/HostGuest]System` classes, enforcing a stronger requirement upfront and removing the need to check for `None` downstream.   
3. Adds a strong symmetry check for consistency of energies, forces, and the *definition* of the potentials themselves. Roughly, we test that `ST(mol_a, mol_b, lamb) == ST(mol_b, mol_a, 1-lamb)`.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `1e1ce2f6b663`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.