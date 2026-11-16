# Issue

**Title:** Neighbor search using tree

## Description

Use `NearestNeighbor.jl` for faster range-based neighbor search.

## ToDos
- [x] real unit tests
- [x] how to set leaf size (sensible default)?

I've tested the code using the following script:
```Julia
using Molly
using BenchmarkTools
using Test

temp = 298
timestep = 0.002
n_steps = 20_000
box_size = 25.0

n_atoms = 40000
coords = [box_size .* rand(SVector{3}) for i in 1:(n_atoms / 2)]
for i in 1:length(coords)
    push!(coords, coords[i] .+ [0.1, 0.0, 0.0])
end
bonds = [HarmonicBond(i, i + (n_atoms ÷ 2), 0.1, 300_000.0) for i in 1:(n_atoms ÷ 2)]

s = Simulation(
    simulator=VelocityVerlet(),
    atoms=[Atom(attype="H", name="H", resnum=i, resname="H", charge=0.0,
                mass=10.0, σ=0.3, ϵ=0.2) for i in 1:n_atoms],
    specific_inter_lists=(bonds,),
    general_inters=(LennardJones(true),),
    coords=coords,
    velocities=[velocity(10.0, temp) .* 0.01 for i in 1:n_atoms],
    temperature=temp,
    box_size=box_size,
    neighbour_finder=DistanceNeighbourFinder(trues(n_atoms, n_atoms), 10, 2.0),
    thermostat=AndersenThermostat(10.0),
    loggers=Dict("temp" => TemperatureLogger(10),
                    "coords" => CoordinateLogger(10)),
    timestep=timestep,
    n_steps=n_steps
)

nf_tree = DistanceNeighbourFinderTree(trues(n_atoms, n_atoms), 10, 2.0)

# Test
find_neighbours!(s, s.neighbour_finder, 0; parallel = true)
ref = copy(s.neighbours)
find_neighbours!(s, nf_tree, 0; parallel = true)
@test s.neighbours == ref

# Benchmark
tree_parallel = @benchmark find_neighbours!(s, nf_tree, 0; parallel = true)
tree_serial = @benchmark find_neighbours!(s, nf_tree, 0; parallel = false)

dist_parallel = @benchmark find_neighbours!(s, s.neighbour_finder, 0; parallel = true)
dist_serial = @benchmark find_neighbours!(s, s.neighbour_finder, 0; parallel = false)
```

which yields the following benchmarks on my machine (2 threads for parallel runs):
```Julia
julia> @show dist_serial
dist_serial = Trial(14.153 s)
BenchmarkTools.Trial:
  memory estimate:  198.97 MiB
  allocs estimate:  80001
  --------------
  minimum time:     14.153 s (0.00% GC)
  median time:      14.153 s (0.00% GC)
  mean time:        14.153 s (0.00% GC)
  maximum time:     14.153 s (0.00% GC)
  --------------
  samples:          1
  evals/sample:     1

julia> @show tree_serial
tree_serial = Trial(2.320 s)
BenchmarkTools.Trial:
  memory estimate:  285.76 MiB
  allocs estimate:  358942
  --------------
  minimum time:     2.320 s (4.27% GC)
  median time:      2.505 s (1.98% GC)
  mean time:        2.505 s (1.98% GC)
  maximum time:     2.690 s (0.00% GC)
  --------------
  samples:          2
  evals/sample:     1

julia> @show dist_parallel
dist_parallel = Trial(11.257 s)
BenchmarkTools.Trial:
  memory estimate:  240.98 MiB
  allocs estimate:  80054
  --------------
  minimum time:     11.257 s (0.00% GC)
  median time:      11.257 s (0.00% GC)
  mean time:        11.257 s (0.00% GC)
  maximum time:     11.257 s (0.00% GC)
  --------------
  samples:          1
  evals/sample:     1

julia> @show tree_parallel
tree_parallel = Trial(1.065 s)
BenchmarkTools.Trial:
  memory estimate:  327.76 MiB
  allocs estimate:  358995
  --------------
  minimum time:     1.065 s (0.00% GC)
  median time:      1.087 s (0.00% GC)
  mean time:        1.136 s (3.35% GC)
  maximum time:     1.303 s (14.62% GC)
  --------------
  samples:          5
  evals/sample:     1
```

I'm sure there are still things to optimize and fine-tune here, but the speed-ups actually look quite good.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `543849cedd65`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.