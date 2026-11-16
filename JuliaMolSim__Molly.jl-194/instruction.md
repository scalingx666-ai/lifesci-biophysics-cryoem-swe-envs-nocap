# Issue

**Title:** Nonbonded kernel

## Description

# Nonbonded force calculation using GPU block-based algorithm

This PR introduces a GPU-optimized algorithm for the calculation of nonbonded forces. The algorithm subdivides the system into blocks of 32 particles, evaluates distances between blocks, and computes forces only for interacting blocks within a cutoff radius. The algorithm is incorporated into OpenMM and it is described in a paper by Eastman and Pande (https://onlinelibrary.wiley.com/doi/full/10.1002/jcc.21413).

This new implementation significantly accelerates nonbonded force calculations for large systems as its performance scales linearly with the number of atoms. 

## Implementation details 

- The algorithm divides the N atoms in bounding boxes of 32. A bounding box is the smallest rectangular, axis-aligned  box that contains 32 atoms. The identification of these boxes is a O(N) problem. 
- For reasons of spatial coherence, a simple Morton code-based reordering algorithm is employed to ensure that atoms close in space are also close to each other in a reoredered array of indices. This is done by diving the 3D space into cubic small voxels with a side length `w` slightly smaller than the cutoff radius. The sequence obtained is used in the functions `kernel_min_max!`, `force_kernel!` and `energy_kernel`. 
 - `kernel_min_max!` : computes the arrays containing minima and maxima of the bounding boxes
- `force_kernel!`: computes the nonbonded forces between particles within interacting bounding boxes
- `energy_kernel`: computes the nonbonded potential energy 

Let `i` and `j` be the indices of the bounding boxes and let `n_blocks` be the allocated number of blocks of GPU threads, which matches exactly the number of bounding boxes. Each couple of bounding boxes forms a tile and the 32 threads allocated in each block compute the interactions within each tile. The structure of `force_kernel!` and `energy_kernel` is divided in 4 main parts:
1) `j < n_blocks && i < j`: computes the forces within 32x32 tiles by shuffling the coordinates, velocities indices of the particles and `Atom` objects  
2) `j == n_blocks && i < n_blocks`: computes the forces between the `r=N%32` particles in the last box and the particles in the other boxes of 32.
3) `i == j && i < n_blocks`: computes interaction within boxes of 32
4) `i == n_blocks && j == n_blocks`: computes interactions within the last box of `r` particles.
   
In cases 1 and 2, for each tile that has been identified as interacting, the distance of each atom in the first bounding box is computed with respect to the second bounding box and if not within the cutoff a flag is set for that atoms to skip the computation. 


## Observations and new neighbor finder (GPUNeighborFinder)

- The algorithm does not require a neighbor list to be passed as an input 
- The computation of the maxima and the minima is performed (by default) every 10 steps of dynamics. However, the user can modify this value by setting explicitly the field `n_steps_reorder` of `GPUNeighborFinder`. 
- `force_kernel!` and `energy_kernel` make explicit use of the matrices `eligible` and `special` defined in `GPUNeighborFinder`



`GPUNeighborFinder`: new type of neighbor finder for systems on GPU. For `GPUNeighborFinder` systems, the function `find_neighbors` returns `nothing` as a neighbor list. This because the new calculation does not require previous knowledge of the neighbor list. The new default choices for the setup of the system are displayed below (lines 890-914 of `setup.jl`):
```
if gpu
    neighbor_finder = GPUNeighborFinder(
        eligible=CuArray(eligible),
        dist_cutoff=T(dist_neighbors),
        special=CuArray(special),
        n_steps_reorder=10,
        initialized=false,
    )
elseif use_cell_list
    neighbor_finder = CellListMapNeighborFinder(
        eligible=eligible,
        special=special,
        n_steps=10,
        x0=coords,
        unit_cell=boundary_used,
        dist_cutoff=T(dist_neighbors),
    )
else
    neighbor_finder = DistanceNeighborFinder(
        eligible=eligible,
        special=special,
        n_steps=10,
        dist_cutoff=T(dist_neighbors),
    )
end
```

A new struct is introduced to store the forces matrix, the arrays of minima and maxima together with the sequence of atom indices ordered according to the Morton code. 
```
struct ForcesBuffer{F, C}
    fs_mat::F
    box_mins::C
    box_maxs::C
    Morton_seq::CuArray{Int32}
end
```

The function `forces_nounits!` takes now a `ForcesBuffer` object as an input. While the forces matrix is updated at each step of dynamics, the minima and maxima arrays and the Morton sequence are only updated every `n_steps_reorder` steps. 

Some tests for this GPU implementation are added in the `test/simulation.jl` and `test/protein.jl`. 


## Performance evaluation
Benchmark of `forces(sys)` performed on `6mrr_equil.pdb` (protein system with 15954 atoms) for the proposed implementation and the previous implementation in Molly: 

|          | `Float32` | `Float64` |
| -------- | -------   | -------   |
| with reordering  | 2.724 ms ± 340.828 μs | 8.018 ms ± 325.429 μs |
| without reordering  | 1.983 ms ± 26.878 μs | 7.134 ms ± 43.339 μs |
| previous implementation (with neighbors calculations)| 15.084 ms ± 399.510 μs |  20.113 ms ± 435.548 |
| previous implementation (without neighbors calculations)| 1.257 ms ± 115.581 μs |  1.585 ms ± 80.793 μs |

The old implementation still outperforms the new one in real systems simulations where the neighbor list is not computed at every step. However, I believe that small fixes can adjust the behaviour of the kernel in long simulations as the benchmark with the reordering seems encouraging. 

The command `potential_energy(sys)` always performs the reordering step in the new implementation since the potential energy will be computed much less frequently than the forces during a simulation.  

|          | `Float32` | `Float64` |
| -------- | -------   | -------   |
| with reordering  | 2.028 ms ± 254.345 μs | 7.783 ms ± 371.269 μs |
| previous implementation | 20.036 ms ± 523.120 μs |  24.720 ms ± 488.714 μs |



A possible setup for a benchmark with `Float64` precision:
```
n_atoms = 16_000
n_steps = 500
temp = 298.0u"K"
boundary = CubicBoundary(6.0u"nm")
coords = place_atoms(n_atoms, boundary; min_dist=0.1u"nm")
velocities = [random_velocity(10.0u"g/mol", temp) for i in 1:n_atoms]
simulator = VelocityVerlet(dt=0.002u"ps")

s = System(
    atoms=[Atom(mass=10.0u"g/mol", charge=0.0, σ=0.05u"nm", ϵ=0.2u"kJ * mol^-1") for i in 1:n_atoms],
    coords=coords,
    velocities=velocities,
    boundary=boundary,
    pairwise_inters=(LennardJones(cutoff=DistanceCutoff(1.0u"nm"), use_neighbors=true),),
    neighbor_finder=DistanceNeighborFinder(
        eligible=trues(n_atoms, n_atoms),
        n_steps=10,
        dist_cutoff=1.2u"nm",
    ),
    loggers=(coords=CoordinatesLogger(100),),
)

s_gpu = System(
    atoms=CuArray([Atom(mass=10.0u"g/mol", charge=0.0, σ=0.05u"nm", ϵ=0.2u"kJ * mol^-1") for i in 1:n_atoms]),
    coords=CuArray(coords),
    velocities=CuArray(velocities),
    boundary=boundary,
    pairwise_inters=(LennardJones(cutoff=DistanceCutoff(1.0u"nm"), use_neighbors=true),),
    neighbor_finder=GPUNeighborFinder(
        eligible=CuArray(trues(n_atoms, n_atoms)),
        n_steps_reorder=10,
        dist_cutoff=1.0u"nm",
    ),
    loggers=(coords=CoordinatesLogger(100),),
)

# CPU 
simulate!(deepcopy(s), simulator, 20; n_threads=1)
@time simulate!(s, simulator, n_steps; n_threads=1)

# GPU
simulate!(deepcopy(s_gpu), simulator, 20; n_threads=1)
@time simulate!(s_gpu, simulator, n_steps; n_threads=1)
```

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `32f5c7326f96`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.