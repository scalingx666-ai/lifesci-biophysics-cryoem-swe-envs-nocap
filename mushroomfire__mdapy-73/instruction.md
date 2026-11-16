# Issue

**Title:** Fix tails of the 2.1 reviews

## Description

Bug fixes found by the reviews of 2.1:

- `Box.minimum_image` gives the shortest vector in boxes of any tilt. It reduces the periodic lattice vectors pairwise and searches the neighboring images, so tilts beyond half a box length no longer give a longer image. The test compares with an LLL-reduced reference in boxes tilted far.
- `Box.wrap` raises `InvalidParameter` for atoms so many boxes away that their image flags do not fit in int64; it gave wrong flags.
- Conditions such as `"element == 'cu'"` refuse texts that are not chemical symbols; they selected nothing. `System` names such symbols when it refuses them.
- The progress of `EAM` and `NEP` counts once from start to end over their passes; it started again at each pass.
- The extended XYZ and LAMMPS dump pages say which types a text file keeps.

Atom ids stay as files give them, repeated or negative, as before: what matches atoms by id already refuses repeated ids.

All 3830 tests pass locally.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `ce0ee2b2f2ad`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.