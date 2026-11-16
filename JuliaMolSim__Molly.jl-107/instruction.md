# Issue

**Title:** NoseHoover Thermostat

## Description

Added NoseHoover thermostat to the simulations file. The code is 95% done and I will add tests soon.

Some questions I have:
- I do not think this could be created as a "coupler" due to the presence of zeta which needs to be updated in the simulation loop, am I missing something?


Unrelated to This Pull Request:
- Is the neighbor list being updated every timestep (looks like it)?
- In the code below why are constraints applied to the copied coordinates? Those coordinates are never used again and not updated by the Integrator.

```
old_coords = copy(sys.coords)
 sys.coords += sys.velocities .* sim.dt .+ ( (remove_molar.(accels_t) .- zeta.*sys.velocities).* sim.dt ^ 2) ./ 2

 apply_constraints!(sys, old_coords, sim.dt)
 sys.coords = wrap_coords.(sys.coords, (sys.boundary,))

```

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `fee9cac1b7da`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.