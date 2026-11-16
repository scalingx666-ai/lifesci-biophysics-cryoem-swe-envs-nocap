# Issue

**Title:** force_constants: fit second order force constants to displaced supercells

## Description

Second step of lattice dynamics on force constants. It builds on #60, whose commits this branch holds until that one merges.

## What it adds

- `force_constants(cell, frames, *, symmetry=None) -> ForceConstants` fits the second order force constants of a supercell to the forces of frames with atoms displaced. The frames can come from any calculator or from DFT. Their atoms are matched to the sites of the supercell by position, so OUTCARs with atoms grouped by element, wrapped, or with another box origin serve as they are. The supercell matrix comes from the box.
- `ForceConstants` gains two attributes: `parameters`, the number of free force constants, and `error`, the relative error of the forces. Both are `None` when the constants are not a fit.
- `displaced_supercells(cell, *, supercell, method="finite", displacement=0.01, count=1, seed=None, symmetry=None)`:
  - `"finite"` makes exactly the moves of `phonons`;
  - `"random"` displaces every atom along a random direction, each pattern by + and −.

## Design

- The supercell-periodic force constants are fitted on the torus, as `phonons` finds them. Wigner–Seitz images stay in `Phonons`.
- Symmetry comes in through the operations that keep the supercell. Each pair orbit is closed under these operations and under the exchange of its two atoms. The free components of a pair are the range of the group-averaged projector of the operations that keep it; spglib's rotations are orthogonalized first.
- The sum rule is exact, by elimination: the fit works in the null space of the summed basis.
- The fit is a dense least squares solve.
  - Frames that leave some free constants undetermined are refused, with the count.
  - A basis over 2 GB is refused too. That happens for crystals of little symmetry with many atoms in a large supercell, which need the cutoff regime of a later PR; `phonons` handles them meanwhile.
- `_phonons.py`: the selection of finite moves is now in `_moves` and `_kept`, shared with `displaced_supercells`. The force constants of `phonons()` are bitwise unchanged, checked for fcc (cubic and primitive), L1₂, hcp and bcc, with and without symmetry.

## How the results are shown to be right

- **Exactly harmonic forces:** forces of known force constants give those constants back to 1e-12 eV/Å² from random patterns, for fcc (primitive and cubic cell), bcc, hcp and L1₂. This holds with symmetry from one pattern, and without symmetry from 3n patterns. A sheet passes the same test.
- **Free parameter counts:** 17, 11, 15, 26 and 31, the counts of symfc.
- **symfc 1.7.3, live:** on the same random displacements and EAM forces (anharmonic, so both are true fits), ours equal symfc's to 1e-12 for all five crystals. symfc is installed with phonopy in CI.
- **Finite moves:**
  - Where the operations turn axes onto axes (fcc, bcc), the fit equals `phonons()` to 1e-11 THz.
  - For hcp and L1₂ the two differ by O(h²): below 3e-4 THz at h = 0.01, and over 50 times smaller at h = 1e-3.
  - The supercells are those `phonons` computes: 1 for fcc and bcc, 2 for hcp, 3 for L1₂, and 6n without symmetry.
- **Order of atoms:** POSCAR files (sorted by element, wrapped) and boxes with another origin give the same force constants.
- **Errors:** every refusal is tested. Coverage of the module is 99.7 %.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `fdbb6423bb5a`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.