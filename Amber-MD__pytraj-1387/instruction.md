# Issue

**Title:** Try to mimic ParmEd API for SimplifiedTopology

## Description

- nested objects
```python
atom.residue.name
atom.residue.atoms
```

- add bond_partners
```python
atom.bond_partners
```

EXPERIMENTAL

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `1f5e803b32e2`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.