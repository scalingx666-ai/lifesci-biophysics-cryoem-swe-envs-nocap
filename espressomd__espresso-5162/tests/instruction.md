# Issue

**Title:** Add interface for ASE calculators

## Description

This extends the ASE plugin for ESPREsSo. Among others, it provideas an integrate() method, which handles the details of transferring data between Espresso and ASE.

At this point, there is a significant problem remaining:
While I was not able to write down a (simple) failing test, a Lennard-Jones liquid simulatinos based on this (sampels/lj_liquid_ase.py) is nto stable (heats up).

One possible cause is that, due to the two-step integrator, cahging particle external forces in every time step, destroys the statistics of the Langevin noise. (The update of particle properties done by ase triggers an invalidation of existing particle forces, which might change the first lj half step)

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `190cda867419`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.