# Issue

**Title:** Interoperability trunk

## Description

- [x] Improve `Topology` construction
  - [x] Fix 
  - [x] Tested in 
- [x] Allow `_SimpleAtom`s` to have names
  - [x]  Test
- [x] Allow unique atom names to be passed through with `_SimpleAtom`s
  - [x] Fix 
- [x] Have `_SimpleMolecule.from_subgraph` bring in metadata
  - [x] Tested in 
- [x] Figure out why `docs/users/pdb_cookbook/index.ipynb` is broken (hopefully unrelated and #1819 fixes it)
- [x] Update docstrings

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `d0e63b9dbb54`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.