# Issue

**Title:** Implement tricilinic cells and virial for ASECalculator

## Description

Support for `TriclinicBoundary` and the virial has been added to ASECalculator. The virial is calculated from the stress given by the ASE calculator. If the calculator does not provide the stress, its virial contribution is ignored with a warning, or an error with `strictness=:error`. This allows NPT simulations with barostats that use the virial, such as `CRescaleBarostat`.

Tests have been added that compare the values obtained from the ASE Lennard-Jones calculator with Molly's `LennardJones` in a triclinic box. A case where stress is not implemented is also tested (TIP3P).

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `32843dcd787e`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.