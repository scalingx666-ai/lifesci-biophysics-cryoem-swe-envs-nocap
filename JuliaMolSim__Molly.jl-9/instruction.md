# Issue

**Title:** Use tuples instead of Dicts

## Description

This adds parametrization on interactions such that all the information about interactions is in the type domain and inferrable.
While the tuple interface is more performant, the old one with dicts can still be used, since the code does not rely on inidexing.
An other performant option would be named tuples, but since the interactions can be distinguished by their types alone, I opted to default to tuples. One possible source of confusion can be that a one element tuple is `(LennardJones(),)` and not `(LennardJones())` (note the `,`).
If you don't want to change the defaults, we could add in the docs somewhere that Tuples will be a bit faster.
```julia
using Molly
using Molly: LennardJones
using BenchmarkTools

n_atoms = 100
box_size = 2.0 # nm
temperature = 298 # K
mass = 10.0 # Relative atomic mass

atoms = [Atom(mass=mass, σ=0.3, ϵ=0.2) for i in 1:n_atoms]
coords = [box_size .* rand(SVector{3}) for i in 1:n_atoms]
velocities = [velocity(mass, temperature) for i in 1:n_atoms]
general_inters = Dict("LJ" => LennardJones())

s = Simulation(
    simulator=VelocityVerlet(),
    atoms=atoms,
    general_inters=general_inters,
    coords=coords,
    velocities=velocities,
    temperature=temperature,
    box_size=box_size,
    thermostat=AndersenThermostat(1.0),
    loggers=Dict("temp" => TemperatureLogger(100)),
    timestep=0.002, # ps
    n_steps=10_000
)

neigh = find_neighbours(s, nothing, s.neighbour_finder, 0, parallel=false)
```
On master:
```julia
@btime accelerations($s, $neigh); # 133.812 μs (5 allocations: 2.58 KiB)
```
On this PR
```julia
# with general_inters = (LennardJones(),)
@btime accelerations($s, $neigh); # 128.410 μs (1 allocation: 2.50 KiB)
```

The improvement is not huge, but it helps reduce allocations to the minimum. The remaining allocation is from creating the forces array of `SVector`s.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `c0331fd19e7f`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.