# Issue

**Title:** [WIP] Implement RMSD restraints

## Description

This is a draft implementation of RMSD restraints to hold the ligand in a defined pose with respect to the receptor.

Until more features are added to OpenMM's `RMSDForce` to allow one atomset for alignment and another atomset for RMSD evaluation, all atoms selected for the receptor and protein are used together for both alignment and RMSD evaluation.

The intention is that if the ligand or protein atoms aren't already specified by the user, all ligand heavy atoms and all receptor alpha carbons (MDTraj DSL `name CA`) are used.

The energy penalty is
```
lambda_restraints * step(RMSD - RMSD0) * (K_RMSD / 2) * (RMSD - RMSD0)^2
```
where the user specifies `RMSD0` and `K_RMSD` if they wish. Sensible defaults are provided.

The intention is for the restraint to be left on throughout the complex phase to define the integration volume for that phase. However, we need to do two more things to make this work correctly:
1. Also use the RMSD restraints for the solvent phase, applied to just the ligand (with the same ligand reference coordinates), where the restraints are turned *on* as the ligand is decoupled
2. Compute the standard state correction by the ratio of the ligand simulation volume to the standard state volume. Anything else is going to be very expensive and not as effective.

For some reason, the tests are failing, but I'm not sure why. 

@andrrizzi @lnaden: If you can take a look, that would be awesome!

I'm not actually sure how to solve issues (1) or (2) with the current code structure, which has grown a little unwieldy (especially the `Boresch` restraint class) for me to be able to easily comprehend how to make those changes.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `5b5363ff24c2`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.