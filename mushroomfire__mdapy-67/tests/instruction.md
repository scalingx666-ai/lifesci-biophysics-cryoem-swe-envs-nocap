# Issue

**Title:** Phonons.thermal_displacements: the mean square displacements

## Description

Part of step (b) of the lattice dynamics plan: harmonic properties beyond the thermodynamics.

## What it adds

`Phonons.thermal_displacements(temperatures, *, mesh=20) -> (T, n, 3, 3)`, in Å².
- It gives ⟨u_α u_β⟩ for each atom of the cell, with the zero-point motion, summed over the modes on a mesh through Γ.
- The diagonal holds the mean square displacements. The Debye–Waller factor is B = 8π²⟨u²⟩: for copper with `EAM.zhou` at 300 K, ⟨u²⟩ = 0.0067 Å² per axis and B = 0.53 Å².
- As in `thermal`, modes below 1e-3 THz (the translations) are left out, and imaginary frequencies are refused.
- Occupations are computed without overflow at low temperatures.

## How the results are shown to be right

- **phonopy 4.8.6 live, on the same force constants** (`run_thermal_displacement_matrices`, mesh 6³ through Γ, `freq_min=1e-3`): fcc, hcp and L1₂ agree at 0, 100, 300 and 900 K to 5.7e-7 relative. That is a constant ratio, phonopy's physical constants; the test tolerance is 1e-5.
- **Closed form:** for simple-cubic springs, the x mode alone moves the atom along x. The result equals the direct mesh sum of ħ(n + ½)/(mω) at 0, 50 and 300 K, and the matrix is isotropic and diagonal.
- **Classical limit:** U doubles from 2000 to 4000 K (1e-3). At T → 0 it is the zero-point motion and positive.
- **Errors:** negative temperatures, bad meshes and unstable crystals are refused. The full suite passes.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `4a1c7c2e3388`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.