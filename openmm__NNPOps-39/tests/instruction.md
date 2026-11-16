# Issue

**Title:** Optimize SpeciesConverter of TorchANI

## Description

This assume that a molecule is constant, so the atomic number conversion to species can be pre-computed.

- [x] Implement `TorchANISpeciesConverter`
- [x] Tests
- [x] Documentation

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `cacb4f8e7b50`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.