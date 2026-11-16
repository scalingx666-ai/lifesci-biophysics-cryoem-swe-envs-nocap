# Issue

**Title:** Resolve circular import

## Description

The import of GAFF in the __init__.py file causes a circular import so that the utils module cannot be imported.

In addition this PR:
- [X] Adds a .gitignore file of python cache and installation files
- [X] Removed maxAttempts setting to be rdkit version agnostic
- [X] Removes PRINT_OPT_PARMAS option that is deprecated in psi4
- [X] Change setup.py to pytroject.toml

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `0ee9ab721053`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.