# Issue

**Title:** Make Velocity Optional when Converting from AbstractSystem

## Description

This PR makes velocity optional when converting form an `AbstractSystem`. Velocity is not a required key I don't think and I currently get errors converting from the example implementation of `FastSystem` to a Molly `System`.

I noticed that we also always zero out the LJ parameters when converting form an AbstractSystem as they are not part of the AtomsBase interface. I'm pretty sure this means if you use the MollyCalculator with AtomsCalculators and LennardJones you will always get 0.0 for forces/energies. Fixing this is a much bigger change as again the sigma/epsilon probably do not belong in the AtomData class.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `44e4dcb01c49`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.