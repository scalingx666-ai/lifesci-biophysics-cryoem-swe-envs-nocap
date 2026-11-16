# Issue

**Title:** Triclinic box support for GPU kernel

## Description

This PR introduces support for triclinic boundary conditions in the nonbonded forces GPU kernel, which previously assumed orthogonal boxes. This enhancement enables simulations that involve: 
- Anisotropic barostats that apply a full pressure tensor.
- Systems with a non-rectangular symmetry (more general boundary conditions)

1. The function `boxes_dist` has been refactored to work in triclinic space by considering the box's basis vectors
```
function boxes_dist(r1_min::SVector{D, T}, r1_max::SVector{D, T}, r2_min::SVector{D, T}, r2_max::SVector{D, T}, boundary::TriclinicBoundary) where {D, T}
    r3 = r2_max - r1_min
    r2 = r3 - boundary.basis_vectors[3] .* round(r3[3] / boundary.basis_vectors[3][3])
    r1 = r2 - boundary.basis_vectors[2] .* round(r2[2] / boundary.basis_vectors[2][2])
    r_a = boundary.basis_vectors[1] .* round(r1[1] / boundary.basis_vectors[1][1])
    a = SVector(abs(r_a[1]), abs(r_a[2]), abs(r_a[3]))
    r3 = r1_max - r2_min
    r2 = r3 - boundary.basis_vectors[3] .* round(r3[3] / boundary.basis_vectors[3][3])
    r1 = r2 - boundary.basis_vectors[2] .* round(r2[2] / boundary.basis_vectors[2][2])
    r_b = boundary.basis_vectors[1] .* round(r1[1] / boundary.basis_vectors[1][1])
    b = SVector(abs(r_b[1]), abs(r_b[2]), abs(r_b[3]))

    return SVector(
        r_a[1] >= zero(T) && r_b[1] >= zero(T) ? zero(T) : ifelse(a[1] < b[1], a[1], b[1]),
        r_a[2] >= zero(T) && r_b[2] >= zero(T) ? zero(T) : ifelse(a[2] < b[2], a[2], b[2]),
        r_a[3] >= zero(T) && r_b[3] >= zero(T) ? zero(T) : ifelse(a[3] < b[3], a[3], b[3])
    )
end
```
2. The logic to compute the maximum and minimum coordinates of a box is updated for triclinic systems. The code uses the inverse of the box matrix $H=(v_1, v_2, v_3)$, which is passed to `kernel_min_max_triclinic!` for efficient execution.
```
if sys.boundary isa TriclinicBoundary
        H = SMatrix{D, D, T}(
           (sys.boundary.basis_vectors[j][i].val for j in 1:D, i in 1:D)...
        )
        Hinv = inv(H) 
        CUDA.@sync @cuda blocks=n_blocks threads=32 kernel_min_max_triclinic!(
                buffers.morton_seq, buffers.box_mins, buffers.box_maxs, sys.coords, Hinv, Val(N),
                sys.boundary, Val(D))
```
3. The kernels `force_kernel!` and `energy_kernel!` were slightly modified to accommodate the new distance and box calculations for triclinic systems.

Additional tests have been added to `test/simulation.jl` for simulations with triclinic boundaries.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `452bbfcf9618`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.