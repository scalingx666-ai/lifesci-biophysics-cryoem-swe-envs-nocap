# Issue

**Title:** Fix dihedral residue option aliases and add coverage

## Description

- Accept residue/residue_selection aliases for phi/psi/omega and combined dihedrals without warnings
- Preserve residue filtering in combined dihedrals and align warning handling with other analyses
- Add tests for residue alias parsing and filtered outputs across dihedral analyses

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `c556bedd4302`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.