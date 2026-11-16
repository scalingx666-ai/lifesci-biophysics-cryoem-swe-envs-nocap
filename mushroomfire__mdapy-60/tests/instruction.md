# Issue

**Title:** ForceConstants: second order force constants and phonopy's FORCE_CONSTANTS

## Description

The first step of lattice dynamics built on force constants: a container for them and the file format of phonopy, so that force constants pass between MDAPY and phonopy (from VASP to MDAPY's phonons, and back). `phonons()` and its numbers are untouched.

## Design

- `mdapy.lattice_dynamics.ForceConstants(cell, supercell, second)`: a frozen container. It holds the cell, the (3, 3) supercell matrix and `second`, which is (n, nS, 3, 3) in eV/Å², the layout of `Phonons.force_constants`. Shapes and values are checked.
  - `phonons()` gives the `Phonons`.
  - `write(path, *, full=False)` writes phonopy's `FORCE_CONSTANTS`. The compact form holds the rows of the atoms of the cell; the full form holds every atom of the supercell.
- `read_force_constants(path, cell, supercell)` reads compact and full files, the same way phonopy does: a header of one number means a square array, and only the first number of the first line of each block is read.
  - When the file is compact on a primitive cell smaller than the cell (phonopy 4 defaults to `PRIMITIVE_AXES = AUTO`), the missing rows of the cell's atoms come from an atom in the file that a translation of the crystal takes to them.
- Phonopy's order of the atoms of a supercell is derived for any supercell matrix: atom by atom of the cell; for each atom, the lattice points of the frame around the supercell with the first vector fastest, each kept at its first copy. MDAPY's matrix is the transpose of phonopy's.

## How the results are shown to be right

- **Order of the atoms:** equal to phonopy 4.8.6's `Supercell` for diagonal and non-diagonal matrices, including fcc→bcc-like and hexagonal ones. Positions agree to 1e-12.
- **phonopy reads our files:** frequencies at 20 random wave vectors agree to 1e-6 relative for fcc, L1₂ and hcp. The compact case uses phonopy's primitive cell `P`; the full case uses `auto`, where phonopy's primitive modes are among the cell's modes at the same wave vector. The millionth is phonopy's conversion factor to THz.
- **We read phonopy's files:**
  - Phonopy's own force constants come from its displacements, with the same EAM potential and h = 2e-5. Read back, they equal those of `phonons()` to 1e-7 eV/Å².
  - This holds compact and full, including the compact file of fcc's one-atom primitive cell for the four-atom cubic cell, where three rows come from translations.
- **Text:** `write` gives the same text as phonopy's `write_FORCE_CONSTANTS`.
- **Round trips:** compact and full, diagonal and non-diagonal, and sheets, all to the 15 decimals of the format.
  - Files with rows left out come back through translations.
  - A crystal where no translation reaches an atom is refused.
- **Errors:** every malformed file raises.
- Coverage of the new module is 100 %.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `97ab492ed0bf`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.