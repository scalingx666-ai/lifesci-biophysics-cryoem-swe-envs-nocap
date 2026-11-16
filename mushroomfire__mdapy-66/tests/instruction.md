# Issue

**Title:** thermal_conductivity: the relaxation-time approximation

## Description

Step (e) of the lattice dynamics plan: the lattice thermal conductivity in the relaxation-time approximation, from the second and third order force constants of (d). It builds on #65 (group velocities) and #64 (third order); this branch holds their commits until they merge. The formulas and conventions follow Fable's prototype, which agreed with phono3py.

## What it adds

- `thermal_conductivity(constants, temperatures, *, mesh, sigma, symmetry=None) -> ThermalConductivity`:
  - three-phonon scattering rates with Gaussians of width `sigma` (THz), on a mesh through Γ;
  - only the irreducible wave vectors are computed, under the rotations that map the mesh onto itself together with time reversal;
  - modes of one frequency share their mean width;
  - κ sums C v⊗v τ over the irreducible points with their weights, v⊗v averaged over the rotations, as phono3py sums it.
- `sigma` is required for now. A later tetrahedron method can make it optional without breaking calls.
- `ThermalConductivity` holds `temperatures`, `kappa` (T, 3, 3) in W/(m K), `q`, `weights`, `frequencies`, `group_velocities`, `gamma` (THz, half the linewidth), `lifetimes` (ps, infinite where a mode does not scatter) and `heat_capacities` (eV/K).
- NumPy only. Copper with a mesh of 11 takes about 2 s; phono3py's C code is faster. A C++ kernel can come later if needed.
- CI installs `phono3py==4.8.2` together with `phonopy==4.8.6` and `symfc==1.7.3`, because a newer phono3py upgrades phonopy. A Linux cp313 wheel exists. The contributing page lists it.

## How the results are shown to be right

- **phono3py 4.8.2 live, the same fc2, fc3, mesh and Gaussian:**
  - **Crystals:** fcc primitive cell (9³), hcp (6×6×4) and L1₂ Cu₃Ni (5³), each at 100, 300 and 600 K.
  - **κ:** every component agrees to about 1e-7 relative (test tolerance 1e-5); the difference is phono3py's older physical constants.
  - **Modes:** the γ of every mode at every one of phono3py's irreducible points, mapped to ours through its star, agrees to 1e-7, and so do the frequencies.
  - **Irreducible points:** the counts equal phono3py's.
- **Symmetry reduction:** the widths at the irreducible wave vectors equal those of the whole mesh to 1e-10, also on a mesh of less symmetry than the crystal (4×4×6 of a cubic crystal). Cubic κ is one number to 1e-10.
- **Scaling:** doubling the third order multiplies γ by 4 and divides κ by 4, to 1e-12. Zero third order gives γ = 0, infinite lifetimes and κ = 0.
- **Classical limit:** from 3000 to 6000 K the widths double and κ halves (1 %), and every mode holds k_B of heat.
- **Errors:** every refusal is tested. Coverage of the module is 100 %, and the full suite passes.

## The page

The page computes the conductivity of copper with `EAM.zhou`, κ(300 K) = 7.5 W/(m K), with a figure of κ(T) and of the lifetimes at 300 K. It explains how to converge the result in `cutoff_third` (11.4 with one or two shells, 7.47 with three, 7.48 within 6 Å), the supercell (7.43 at 5³), the `mesh` (7.71 at 15³) and `sigma`. It notes that this is the phonon part only (electrons carry about 400 W/(m K) in copper), and what happens without spglib. It also has the definition and the references (Togo et al. 2015, 2023).

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `5f1840f5b44f`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.