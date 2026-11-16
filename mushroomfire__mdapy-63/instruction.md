# Issue

**Title:** bond_stiffness: springs along and across bonds, as functions of their length

## Description

Step (c) of the lattice dynamics plan: the bond stiffness against bond length model (van de Walle and Ceder 2002; Wu, Ceder and van de Walle 2003), the 2.x form of 1.x's `BondStiffness`. It builds on #62 (cutoff regime) and #61; this branch holds their commits until they merge.

## What it adds

- `bond_stiffness(data, *, cutoff, degree=1) -> BondStiffness`.
  - `data` is a sequence of `(cell, frames)` for any number of crystals: pure metals, ordered compounds, small random alloys, each at several sizes. Frames come from any calculator or from DFT (OUTCARs), matched by position as in `force_constants`.
  - Every frame enters one least-squares fit of the forces. It fits springs along (k_l) and across (k_t) each bond, as polynomials of the bond length for each pair of elements in each shell.
  - `cutoff` is the longest bond, or the outer radii of shells, `(3.0, 4.0)`. Each shell has its own polynomials, as 1.x had them per shell; the radii are given, not guessed from a tolerance.
- `BondStiffness` holds `pairs` (element, element, shell), `shells`, `degree`, `longitudinal`, `transverse` (coefficients of powers of r), `lengths` (the range fitted) and `error`. It offers two methods:
  - `stiffness(first, second, length)`;
  - `force_constants(cell, supercell=1) -> ForceConstants`, the springs of any structure, such as a relaxed SQS, and so its phonons and vibrational free energy.
- The pair table and the design matrix are shared with `force_constants` (refactored into `_pair_table`, `_design`, `_supercell_of`).

## How the results are shown to be right

- **Exact springs:** forces exactly −Φu of known springs, for Cu, Ni and L1₂ at three sizes plus a 32-atom alloy, with two shells. The coefficients come back to 1e-10 and the error is below 1e-12. A degree of 2 returns the same springs with the quadratic term at 0.
- **Independent pair potential** (a test calculator, φ = C r² + D r³, hard cutoff between shells), ± patterns of 0.005 Å: k_l = φ″ = 2C + 6D r and k_t = φ′/r = 2C + 3D r to 1e-4 relative, the cube of the displacement. This fixes the sign and the convention.
- **Force constants of the springs:** checked block by block against the formula. The self term is minus the sum of the bonds, the blocks are zero beyond the last shell, and the sum rule and the pair symmetry hold.
- **Errors:** every refusal is tested, including a pair with too few lengths for the degree and a missing pair in a shell at prediction. Coverage of the module is 100 %. The full suite passes, also on Python 3.11.

## The page

The page fits Cu–Ni with `EAM.zhou` on the pure metals, both L1₂ and a small alloy at three sizes; the force error is 2.7 %. It then predicts a 108-atom random alloy that no fit held. At 300 K the springs give S = 3.496 k_B per atom against 3.479 from the full phonons, which the page shows as the way to check the springs on one structure. It has a figure of k_l and k_t against bond length. Both papers are on the references page and in a "Please cite" box. The migration guide now says `BondStiffness` is `bond_stiffness`.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `e17609e0c72f`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.