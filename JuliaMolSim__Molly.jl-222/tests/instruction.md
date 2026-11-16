# Issue

**Title:** Added soft-core potentials (Beutler et al. & Gapsys et al.)

## Description

I have added four functions of which two replace the previous soft-core potentials (LennardJonesSoftCore and CoulombSoftCore). 

- Added the soft-core potentials described in Beutler et al. (1994) in Chem. Phys. Lett. and called these functions LennardJonesSoftCoreBeutler and CoulombSoftCoreBeutler, these were similar to the previous soft-core potentials, but I reversed the λ-scaling so that for λ =1, the interaction is fully turned on and for λ=0, the interaction is turned off.
- Added the soft-core potentials described in Gapsys et al. (2012) in JCTC and called these functions LennardJonesSoftCoreGapsys and CoulombSoftCoreGapsys.
- Added a new atom type 'Atom_L' in preparation for atoms that will be controlled with λ. (Essentially the same as the 'Atom' type but with an added λ variable). However, it is not being used yet.
- Updated the test/interactions.jl to include tests for these functions.
- Updated the test/simulation.jl to run with LennardJonesSoftCoreBeutler in the HREMD.
- Updated the soft-core potentials section in examples.md to showcase the difference between these two types of soft-core potentials and the effect of λ on both.
- Updated documentations.md where soft-core potentials were mentioned.
- Updated cutoffs.jl by exporting the force_cutoff function to enable plotting the magnitude of the forces. While it is not used in the examples.md, it was helpful when I was plotting the forces generally.

Important note: These interactions are for appearing and disappearing atoms only, not for transformations between different atoms (e.g. a alchemical transformation from carbon to hydrogen atom).

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `929e02fddce9`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.