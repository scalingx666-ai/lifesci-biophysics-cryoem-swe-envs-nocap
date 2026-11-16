# Issue

**Title:** Phonons.gruneisen: mode Grüneisen parameters

## Description

The rest of step (b) of the lattice dynamics plan. It builds on #67 (thermal displacements); this branch holds its commit until it merges.

## What it adds

`Phonons.gruneisen(q, *, smaller, larger) -> (3n,) or (K, 3n)`: γ = −(V/ω) dω/dV, computed as −(V/2ω²) eᴴ (D₊ − D₋)/(V₊ − V₋) e.
- Inputs are the phonons of the same crystal at a smaller and a larger volume, such as the cell scaled by ±1 %.
- Nearly degenerate modes are grouped by eigenvalue closer than 1e-4 eV/(Å² amu), as phonopy's `lift_degeneracy` does, and set apart in the basis that diagonalizes the change.
- Modes below 1e-3 THz get NaN.
- Phonons of other atoms, or volumes that do not bracket this one, are refused.

## How the results are shown to be right

- **phonopy 4.8.6 live:** `GruneisenBase` on the same force constants at three volumes agrees to 1e-10 for fcc primitive and cubic cells, hcp and L1₂, at 13 wave vectors including degenerate ones. No physical constants enter, so the agreement is exact.
- **Closed form:** for simple-cubic springs at rest, the mode along x has γ = −(2 − cos 2πq_y − cos 2πq_z)/(6(1 − cos 2πq_x)) (only the tension of the bonds changes). This holds to 1e-5 at ±1e-4 in volume.
- **Errors:** Γ gives NaN, and the refusals are tested. The full suite passes.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `4e43a14d276b`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.