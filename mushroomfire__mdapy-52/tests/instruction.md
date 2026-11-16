# Issue

**Title:** orthogonal_cell: a crystal in a box along x, y and z

## Description

The first of the 1.x features promised on the migration page. `mdapy.transform.orthogonal_cell(system, *, minimal=False, max_search=20, tolerance=1e-6)` puts a crystal in a box whose lattice vectors lie along x, y and z. Along each axis it takes the shortest combination of the box's lattice vectors that lies along it, as atomsk's `-orthogonal-cell` does.

**Design**
- The new box is a supercell of the lattice. It is filled exactly with the copies at the lattice points inside it, so no duplicates have to be removed.
- `minimal=True` starts from the primitive cell (`primitive_cell`), which gives the smallest orthogonal cell. By default it starts from the box, as atomsk does.
- Ids, molecules, image flags and info follow `replicate`.
- Topology terms join the copies where their atoms lie next to the term's first atom: by the image flags, or else at the nearest image, as in #46's `replicate`. A molecule across the old boundary therefore keeps its bonds.
- A box that is already orthogonal comes back as it is.
- A crystal without a lattice vector along an axis raises `InvalidParameter`, as atomsk fails too.

**How the results are shown to be right**
- **Live against atomsk** on four restricted-triclinic boxes, in their unit and 2×3×1 forms, with three elements at random positions: the same box and the same atoms in each case.
- **Analytic checks**
  - hcp gives 4 atoms in a, a√3, c;
  - wurtzite gives 8;
  - the primitive cells of fcc and rock salt give their cubic cells;
  - `minimal=True` of a 2×3×2 hcp box gives the 4-atom cell.
- **Invariants on random crystals:** the volume per atom is unchanged; every atom sits on a lattice site of its element; the copies sit at distinct lattice points.
- **Columns, ids, image flags and topology:** bonds and an angle across the boundary keep their vectors, with and without image flags.

Covered 99 %. The page is user-guide/build/orthogonal-cell.md, with a picture.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `40443b958c86`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.