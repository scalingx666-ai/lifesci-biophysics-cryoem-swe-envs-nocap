# Issue

**Title:** Use PhiloxRNG

## Description

This PR uses PhiloxRNG for generating noise for `random_velocities`, `apply_Andersen_coupling_kernel!`, and `Langevin` and `LangevinSplitting` simulation as a  

Here is a benchmark for Langevin dynamics without any forces.

```julia
using Molly
using CUDA
using Random
using StaticArrays

function runbench(; ftype::Type{T}=Float32, atype::Type{AT}=Array, n_atoms = 100000) where {T, AT}
    atom_mass = 10.0f0u"g/mol"
    box_len = T(cbrt(n_atoms/100)*2.0)*u"nm"
    boundary = CubicBoundary(box_len)
    temp = 100.0f0u"K"
    atoms = AT([Atom(mass=atom_mass, σ=0.3f0u"nm", ϵ=0.2f0u"kJ * mol^-1") for i in 1:n_atoms])
    coords = AT([rand(SVector{3,T})*box_len for i in 1:n_atoms])
    velocities = AT([random_velocity(atom_mass, temp) for i in 1:n_atoms])
    simulator = Langevin(;
        dt=0.002f0u"ps",
        temperature=temp,
        friction=T(1.0)u"ps^-1",
    )

    sys = System(
        atoms=atoms,
        coords=coords,
        boundary=boundary,
        velocities=velocities,
        # loggers=(
        #     temp=TemperatureLogger(typeof(1.0f0u"K"), 10),
        # ),
    )

    simulate!(deepcopy(sys), simulator, 20; show_progress=false) # Compile function
    n_iters = 10_000
    (CUDA.@elapsed simulate!(sys, simulator, n_iters; show_progress=false))/n_iters
end

for atype in (Array, CuArray)
    for n_atoms in (10_000, 100_000)
        for ftype in (Float32, Float64)
            println("$n_atoms atoms $ftype $atype")
            println(round(Int, runbench(;n_atoms, ftype, atype)*1E6), " μs/it")
            println()
        end
    end
end
```

Results PR vs master:

Julia Version 1.12.6
CPU: 16 × AMD Ryzen 7 9800X3D 8-Core Processor
NVIDIA GeForce RTX 3080

10000 atoms Float32 Array
93 μs/it vs 170 μs/it

10000 atoms Float64 Array
144 μs/it vs 192 μs/it

100000 atoms Float32 Array
905 μs/it vs 1723 μs/it

100000 atoms Float64 Array
1373 μs/it vs 1915 μs/it

10000 atoms Float32 CuArray
31 μs/it vs 158 μs/it

10000 atoms Float64 CuArray
88 μs/it vs 190 μs/it

100000 atoms Float32 CuArray
42 μs/it vs 1148 μs/it

100000 atoms Float64 CuArray
174 μs/it vs 1254 μs/it

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `8d5fadd371d9`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.