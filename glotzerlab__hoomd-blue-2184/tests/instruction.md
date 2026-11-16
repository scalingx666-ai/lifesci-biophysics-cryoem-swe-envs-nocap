# Issue

**Title:** Update for frictional contacts

## Description

## Description

- I updated the Documentation to align with the style general style.  

- The calculation of the frictional force has been modified. It is now based exclusively on the z-component of the moment of inertia. This change introduces support for 2D simulations, which can be configured by using a moment of inertia of [0, 0, I]. For 3D simulations, the system requires isotropic spheres, specified by an identical moment of inertia in all dimensions (e.g., [I, I, I]).

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `f5d00ece6674`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.