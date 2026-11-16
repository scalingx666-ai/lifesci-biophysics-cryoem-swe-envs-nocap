# Issue

**Title:** Implement cubic spline cutoff

## Description

## Cubic Spline implementation

This is a suggestion to add a spline cutoff, which interpolates between the true potential at a set activation distance and zero at a cutoff distance. The interpolation itself is based on a [cubic Hermite spline](https://en.wikipedia.org/wiki/Cubic_Hermite_spline).

# Main addition
The main addition consists in a new cutoff type, `CubicSplineCutoff` :
```
struct CubicSplineCutoff{D,S,I}
    dist_cutoff::D
    sqdist_cutoff::S
    inv_sqdist_cutoff::I

    dist_activation::D
    sqdist_activation::S
    inv_sqdist_activation::I
end
```
This cutoff comes with two methods 

`force_divr_cutoff(cutoff::CubicSplineCutoff, r2, inter, params)`
`potential_cutoff(cutoff::CubicSplineCutoff, r2, inter, params)`

which define the behaviour of the cutoff within the cutoff region, between `dist_activation` and `dist_cutoff` 

# Small adjustments
The naming convention used above required changing some of the code in the "interactions" folder, specifically the `potential_energy` and `force` methods defined in coulomb.jl, lennard_jones.jl, mie.jl and soft_sphere.jl. This is because originally, these methods enforce the cutoff in the case of two cutoff points (as is the case here) if the squared distance between two atoms is between cutoff.activation_dist and cutoff.sqdist_cutoff, which is misleading.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `24ee99950e2b`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.