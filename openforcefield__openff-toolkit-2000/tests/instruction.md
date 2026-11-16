# Issue

**Title:** Improve some typing

## Description

This PR includes many changes needed for OpenFF Pablo to type check, and 1 for it to run correctly. Pablo development so far has and will continue to test and type check against this branch (or `main` once it is merged) until it gets into a release.

- [x] improves typing as needed by OpenFF Pablo by adding and correcting many type annotations
- [x] Handle an exception previously raised when `Molecule.to_rdkit()` is run on a molecule with residue numbers that cannot be converted to `int` (such as hexadecimal residue numbers used by OpenMM in large PDB files)
- [x] Makes `Molecule.is_isomorphic_with()` so that its keyword arguments are actually keyword arguments. This is not a breaking change as the `**kwargs` argument is still present, it's just ignored - we should deprecate and remove this ASAP 
- [x] Update [changelog](https://github.com/openforcefield/openff-toolkit/blob/main/docs/releasehistory.md)


Sorry for the behemoth PR!

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `d5d4cf8e4267`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.