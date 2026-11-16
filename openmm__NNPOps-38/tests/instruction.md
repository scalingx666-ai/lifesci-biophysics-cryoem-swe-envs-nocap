# Issue

**Title:** Optimize EnergyShifter of TorchANI

## Description

This assume that a molecule is constant, so the energy contribution from the atomic self-energies can be pre-computed.

- [x] Implement `TochANIEnergyShifter`
- [x] Test
- [x] Documentation

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `6939536c9958`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.