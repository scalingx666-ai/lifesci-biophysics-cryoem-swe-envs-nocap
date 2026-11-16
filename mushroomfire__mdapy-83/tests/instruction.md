# Issue

**Title:** md_elastic_constants: method="fluctuation"

## Description

Builds on #82, whose commit it contains until that merges.

`md_elastic_constants(..., method="fluctuation")` gives the elastic constants from the fluctuations of the stress in one run at the mean box. The usual NPT run finds the box first, then one Langevin NVT run at that box gives the constants, with no strains (`strain` is unused):

C = ⟨C^B⟩ − V/(k_B T) cov(σ_virial) + (N − 1) k_B T / V (δ_ik δ_jl + δ_il δ_jk)

- **Born term**: LAMMPS `compute born/matrix numdiff`, strains of 1e-5, recorded every ten stress records (about 15 % more time). EAM, Tersoff, SW and NEP have no analytic Born term, and 1e-5 is right from EAM to NEP; 1e-6 is not, for NEP.
- **Virial stress**: `compute pressure NULL virial`, every `every` steps.
- **Kinetic term**: the momenta of all atoms but the center of mass, which the thermostat keeps still, as LAMMPS counts the temperature.
- **Under a mean stress**: Wallace's term makes them the stress–strain coefficients that the strain method gives; under P, that is C11 − P, C12 + P and C44 − P.
- **Errors**: from `blocks` blocks of every term.
- **Frame**: the Born tensor goes from the frame of LAMMPS to that of the system with the same rotation as the stress.
- **Sheets**: the in-plane components, times the height of the box, in N/m.

**Against the strain method** (Triton, the shipped function, 300 K, 300 000 steps):

| | C11 | C12 | C44 |
|---|---|---|---|
| Cu, Mishin EAM, 864 atoms, 2 seeds | 163.75–163.81 ± 0.1 | 119.86–119.90 | 71.69–71.71 |
| the strain method after #82, 4 seeds | 163.79–164.03 ± 0.3–0.4 | 119.89–120.03 | 71.69–71.76 |
| Si, Tersoff, 216 atoms, 2 seeds | 109.38–109.98 ± 0.2 | 75.63–76.31 | 8.7 ± 2.0, 8.9 ± 1.1 |
| the strain method after #82 | 109.29–109.53 ± 0.5–0.6 | 75.49–75.91 | 9.71–9.94 ± 0.2 |
| Cu, 256 atoms, 10 GPa | 207.01–207.05 | 154.41–154.45 | 90.44–90.49 |

Under 10 GPa the strain method (with the old thermostat) gave 207.11(25) / 154.91(12) / 90.78(13). Its C12 is 0.5 GPa higher, which may be the nonlinearity of its 1 % strain; that is not settled.

The study behind this, with a prototype, ran 5–1800 K and 256–2048 atoms on copper, plus the liquid. At equal steps the fluctuations give errors 1.5–6 times smaller for C11 and C12 and 4–25 times for C44, independent of the size. Where atoms relax strongly within the cell, as the shear of silicon (92 GPa unrelaxed, 10 relaxed), they are noisy. The page "Elastic constants from molecular dynamics" gets a section with the formula and a table of when each method fits: fluctuations for metals and cells of small internal relaxation, small cells, low temperatures and liquids; strains for strong internal relaxation and large cells.

**Tests** (`tests/lattice_dynamics/test_md_elastic.py`, `test_sheets_lammps.py`):
- At 1 K the static constants (5e-4).
- The strain method on the same hot crystal, within 4 combined σ.
- An ideal gas (`pair_style zero`) under 0.05 GPa gives B11 = B12 = P and B44 = 0 within the errors. This checks the kinetic term and the term under stress analytically.
- Graphene at 1 K gives the static constants in N/m within the errors (its internal relaxation, 22 N/m of C11, makes them ±3).
- An unknown method is refused.

The docs and API tests pass, and the related suites (lattice dynamics, docs, API) pass locally: 485 tests.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `284c2bd9b19d`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.