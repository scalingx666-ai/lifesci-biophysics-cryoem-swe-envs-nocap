# Issue

**Title:** Fix alchemical Ions were part of ligand

## Description

Topography ligand_atoms` and solvent_atoms` were returning the internal object, meaning counter-ions added on behalf of the ligand were being treated as the ligand in `top.ligand_atoms`

@andrrizzi and I have determined this should not have affected any of the SAMPL or APR calculations we have, however, this will cause the center of mass in Harmonic based restraints to be shifted slightly. Unbiasing the restraint corrects for this.

The fix is that these two properties now return copies of the internal objects they are protecting. The trade off is you will be unable to set `top.ligand_atoms[i] = x`, although this will not raise an error. You will need to set the whole property with `top.ligand_atoms = X` instead.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `c0f402d80937`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.