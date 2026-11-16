# Issue

**Title:** Fix TER atom counting in amber trajectory processing

## Description

This pull request updates the logic for handling atoms in the Amber trajectory reader, specifically to exclude "TER" pseudo-atoms from atom counting and copying operations. This ensures that only real atoms are processed, improving accuracy and consistency in the code.

Atom handling improvements:

* Updated the atom counting logic to skip atoms marked as `Ter` (pseudo-atoms) when tallying the number of atoms in a residue, ensuring only real atoms are counted.
* Modified the atom copying logic to skip `Ter` pseudo-atoms when creating new atom objects from template atoms, preventing unnecessary or incorrect copying of pseudo-atoms.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `80e5d42cd1d2`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.