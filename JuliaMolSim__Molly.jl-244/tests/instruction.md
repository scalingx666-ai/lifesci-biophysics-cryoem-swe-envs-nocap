# Issue

**Title:** feat: Add dissipative particle dynamics (DPD) support

## Description

Implement DPDInteraction and DPDVelocityVerlet for mesoscale simulations.

DPDInteraction is a pairwise interaction combining conservative (soft repulsion), dissipative (velocity-dependent drag), and random (pairwise- correlated noise) forces following Groot and Warren 1997. Deterministic per-pair Gaussian noise is generated via a hash of particle indices and step number, ensuring momentum conservation and thread safety without shared mutable state.

DPDVerlet implements the Groot-Warren modified velocity-Verlet integrator, which adds a velocity prediction step (v_predicted = v(t) + λ*dt*a(t)) before recomputing forces at the new positions. This is necessary because dissipative forces depend on relative velocities, which standard velocity- Verlet handles poorly.

Tests cover force components, dissipative damping behaviour, noise symmetry, and thermal equilibration to the target temperature.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `6c7887a459c7`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.