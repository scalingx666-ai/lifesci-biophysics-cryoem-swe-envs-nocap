# Issue

**Title:** Phonons.group_velocities: the velocities of the modes

## Description

Part of step (b) of the lattice dynamics plan, and what the RTA thermal conductivity (e) needs. It builds on #64; this branch holds its commits until #63 and #64 merge.

## What it adds

`Phonons.group_velocities(q, *, symmetry=None) -> (3n, 3) or (K, 3n, 3)`, in Å/ps (THz·Å, phonopy's unit), with the modes in the order of `frequencies`.

- **Method:** v = dω/dk = π c²/ν · ⟨e|∂D/∂k|e⟩, from the analytic derivative of the dynamical matrix over the same Wigner–Seitz images as `Phonons` uses.
- **Equal frequencies:** modes within 1e-4 THz of each other are set apart in the basis that diagonalizes ∂D along (1, 2, 3).
- **Symmetry:** velocities are averaged over the Cartesian rotations of the crystal that keep the wave vector itself, with no reciprocal lattice vector. This is phonopy's convention, which (e) needs so that κ keeps the symmetry of the crystal.
- **Low frequencies:** modes below 1e-4 THz have zero velocity.

## How the results are shown to be right

- **phonopy 4.8.6 live, on the same force constants:** fcc, hcp and L1₂ agree to 1.2e-7 relative at 10 random wave vectors and 9 special ones, where modes are degenerate (Γ, X, M, L, K, Γ–A, …). The 1.2e-7 is a constant ratio, phonopy's older physical constants; the test tolerance is 1e-6.
- **Closed form:** for springs of a simple cubic lattice, v = a√(k/m) cos(k_α a/2) along the axis of each mode, to 1e-6 Å/ps.
- **Finite differences** of the frequencies agree to 1e-6 Å/ps for fcc and hcp at random wave vectors, without symmetry.
- **Symmetry:**
  - Γ gives zero;
  - hcp along Γ–A gives velocities along z, with each degenerate pair equal;
  - a sheet gives velocities in its plane.
- The full suite passes, also on Python 3.11.

The Phonons page documents the method and the conventions.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `f6e2acb8ebeb`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.