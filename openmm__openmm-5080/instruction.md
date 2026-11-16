# Issue

**Title:** Cleanup TinkerReader, ForceField, and amoebaforces

## Description

As discusses in #4769, this PR serves to reduce the duplicated code between `ForceField` and `TinkerFiles`. 
Current progress checklist for the refactoring:

- [x] AmoebaBondForce
- [x] AmoebaAngleForce
- [x] AmoebaOutOfPlaneBendForce 
- [x] AmoebaInPlaneAngleForce
- [x] AmoebaStretchBendForce
- [x] ~~- AmoebaTorsionForce~~ (deprecated in favour of `PeriodicTorsionForce`)
- [x] AmoebaPiTorsionForce
- [x] AmoebaUreyBradleyForce
- [x] AmoebaWcaDispersionForce
- [x] AmoebaGeneralizedKirkwoodForce
- [x] AmoebaMultipoleForce
- [x] AmoebaVdwForce
- [x] AmoebaTorsionTorsionForce
- [x] AmoebaAngleTorsionForce 
- [x] AmoebaStretchTorsionForce 

We should also consider updating https://docs.openmm.org/latest/userguide/library/08_amoeba_plugin.html to reflect the current state of the AMOEBA code.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `939e0af545bb`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.