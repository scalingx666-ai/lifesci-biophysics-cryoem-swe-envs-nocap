# Issue

**Title:** Example: lattice dynamics of solid argon

## Description

The examples chapter for the force constants work: one page that runs the whole workflow end to end on solid argon (Lennard-Jones in LAMMPS). It builds on #71 (Green–Kubo); this branch holds the commits of #69–#71 until they merge.

## Contents

1. **The crystal:** LJ argon (ε = 10.4 meV, σ = 3.405 Å, cut at 8.5 Å), relaxed, and its primitive cell.
2. **Phonons:** one finite-displacement supercell (5³) gives the 26 free force constants. The page shows the bands and DOS figure and writes FORCE_CONSTANTS for phonopy.
3. **How the atoms move:**
   - thermal displacements and Debye–Waller factors at 0–80 K (B = 0.86 Å² at rest, 3.12 Å² at 80 K);
   - mode Grüneisen parameters from ±1 % volumes (mean 3.0; 2.7–3.4 above 0.2 THz). The page explains why the acoustic modes nearest Γ are left out.
4. **Springs along the bonds:** `bond_stiffness` over five shells at three sizes gives back φ″ and φ′/r of the potential to a few parts in a thousand. The figure plots the fitted springs over the analytic derivatives.
5. **Thermal conductivity:**
   - third order from ± random patterns (85 parameters, 0.24 % force error);
   - RTA κ(T) at 20–80 K (1.47 → 0.43 W/(m K));
   - Green–Kubo at 20, 40 and 80 K (1.23 ± 0.2, 0.66 ± 0.12, 0.35 ± 0.06), in one figure;
   - the text explains why the phonons carry somewhat more: four-phonon and higher scattering, quantum against classical occupations, and the size of the box.

## Checks

- The page runs as a test in 25 s, so the doc-examples job runs it; `needs: lammps` skips it where LAMMPS is not installed.
- Every number in its comments comes from the run.
- Three pictures were rendered from the page by `tools/doc_images.py`.
- The Force constants page links to the example, and the release notes list it.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `77ab906011cf`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.