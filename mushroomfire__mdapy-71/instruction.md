# Issue

**Title:** green_kubo: the conductivity of the fluctuations of the heat flux

## Description

Step (f) of the lattice dynamics plan, the last one: the thermal conductivity by the Green–Kubo relation. It builds on #70; this branch holds its commits until #69 and #70 merge.

## What it adds

- **`green_kubo(system, potential, temperature, *, timestep, equilibration, production, every, correlation, blocks, seed)`** runs equilibrium MD in LAMMPS:
  - Nosé–Hoover at the temperature, then constant energy;
  - while running, it records `compute heat/flux` with `centroid/stress/atom`, which many-body potentials such as EAM need (Surblys et al. 2019), at full precision;
  - a potential without centroid stress (Stillinger–Weber, Tersoff) is refused with a clear message instead of a silently wrong flux;
  - the temperature of the result is the mean of the constant-energy run.
- **`green_kubo_from_flux(flux, *, interval, volume, temperature, correlation, blocks=5)`** does the same with a flux of any MD (J·V in eV Å/ps, as LAMMPS gives it in metal units):
  - each lag is the mean over every time origin (FFT), the correlation is made symmetric in α, β, and integrated by trapezoids;
  - the standard error comes from blocks of the flux.
- **`GreenKubo`** holds `temperature`, `volume`, `times`, `running` (C, 3, 3), `kappa`, `errors` and `flux`.

## How the results are shown to be right

- **LAMMPS's own Green–Kubo, live:** in the same run of LJ argon, LAMMPS's `fix ave/correlate … type auto ave running` with `trap()`, as in its examples/KAPPA, and our integral of the flux it samples agree to 1e-12 relative. This pins the units, the normalization of each lag and the integration.
- **Brute force:** the correlation and running integral equal a direct NumPy sum over time origins to 1e-10.
- **A process of known correlation:** an Ornstein–Uhlenbeck flux of variance s² and time τ gives s²τ·(V/k_B T²) within 5 % on the mean of the axes. Each axis lies within 4 of the block errors, which are of the right size.
- **Physical sanity:** solid LJ argon at 40 K gives 0.75 ± 0.14 W/(m K) (500 atoms, 800 ps). The RTA of #66/#70 from force constants of the same potential gives 0.81.
- **Errors:** every refusal is tested, including Stillinger–Weber for the missing centroid stress. Coverage of the module is 98 %. The full suite passes.

## The page

It has an example of solid argon with a picture of the running integral (rise, plateau, noise). It explains how to choose `correlation`, the errors and size effects, the comparison with the RTA and why the two differ, how to use a flux from other codes, and the definition. References: Schelling et al. 2002 and Surblys et al. 2019.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `c1188ef7ed8f`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.