# Issue

**Title:** Assign ELCIC mirror forces to the correct particles

## Description

I've stumbled upon another subtle bug in ELCIC force calculation.

When we add the `dielectric_layers_contribution(...)`, we mirror the particle1 position and calculate the distance to particle2. However, we then assign the force to particle 1, which is wrong - it's the force on particle 2.

<img width="564" height="530" alt="elc_image_force_bug" src="https://github.com/user-attachments/assets/3bd0af40-9826-48bc-9904-62957fc4e691" />

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `c0127f530d63`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.