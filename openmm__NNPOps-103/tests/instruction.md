# Issue

**Title:** Particle Mesh Ewald

## Description

I'm working on an implementation of PME.  This will be useful for models that include explicit Coulomb terms, especially when the charges are predicted by the model and require extra chain rule terms.

The direct space interaction is implemented in PyTorch and is very simple.  The reciprocal space part is implemented as a C++ extension.  I've written the CPU version.  I still have to do the CUDA version.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `054d487d9fec`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.