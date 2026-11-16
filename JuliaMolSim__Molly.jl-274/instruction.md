# Issue

**Title:** Minor PME CPU Improvements

## Description

Looked for some quick changes to speed up PME on CPU. The `ustrip` optimization probably could be used for the GPU path as well. I just don't have any easy way to test that right now. I do not really see any way to improve the k-space as that just calls FFTW. On the real-space side I made the following changes:

1. `ustrip` `α`, volume, `recip_box`, and `f/ϵr` once outside the k-space loop, and run convolution without units. Apply units at the end.
2. Hoisted some calculations in charge spreading and force interpolation. This probably has modest improvements, but it was definitely wasted FLOPs.
3. Pass `num_threads` to fft plans (EDIT)

### Result (6mrr, ~16k atoms, 4 Julia threads)
Timing `Molly.ewald_pe_forces!` only, 1000 iterations after warmup:

≈ **16% faster** on the PME energy+forces path single thread

### Benchmark Code
```julia
using Molly

n_threads = 1
ff = MolecularForceField(
    Float64,
    joinpath(pkgdir(Molly), "data", "force_fields", "ff99SBildn.xml"),
    joinpath(pkgdir(Molly), "data", "force_fields", "tip3p_standard.xml"),
)
sys = System(
    joinpath(pkgdir(Molly), "data", "6mrr_equil.pdb"),
    ff;
    array_type=Array,
    nonbonded_method=:pme,
    center_coords=false,
)
pme = only(filter(i -> i isa PME, sys.general_inters))
Fs = Molly.zero_forces(sys)

# warmup
for _ in 1:3
    fill!(Fs, zero(eltype(Fs)))
    Molly.ewald_pe_forces!(Fs, nothing, sys, pme, Val(false); n_threads=n_threads)
end

n = 1000
samples_ms = Vector{Float64}(undef, n)
for i in 1:n
    fill!(Fs, zero(eltype(Fs)))
    t0 = time_ns()
    Molly.ewald_pe_forces!(Fs, nothing, sys, pme, Val(false); n_threads=n_threads)
    samples_ms[i] = (time_ns() - t0) / 1e6
end
μ = sum(samples_ms) / n
se = sqrt(sum((x - μ)^2 for x in samples_ms) / (n - 1)) / sqrt(n)
println("ms / ewald_pe_forces! = $(round(μ; digits=3)) ± $(round(se; digits=3))")
```

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `4fc5518d7742`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.