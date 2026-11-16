# Issue

**Title:** Add custom Boltzmann's constant support

## Description

This PR proposes letting the user specify a Boltzmann's constant when defining a System, allowing for reduced unit systems.
The changes are the following

**src/types.jl:**
Add a `k` field and `K` type parameter to the `System` struct
Add logic for default behavior

 **src/spatial.jl:**
Let `AtomsBase.velocity`, `velocity3D`, `velocity2D` and `maxwell_boltzmann` take an additional parameter `k`
modify `random_velocities` to use the system's boltzmann constant
 
**src/coupling.jl:**
Fix velocity resampling to rely on the system's `k`

**src/temperature.jl:**
Modify `temperature` to convert kinetic energy to temperature using on the system's `k`

**test/basic.jl**
Modify basic tests of velocity generation

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `0d604d5e29ca`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.