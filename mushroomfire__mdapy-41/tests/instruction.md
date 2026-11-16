# Issue

**Title:** insert_dislocation: no fault when a plane of atoms lies just below the cut

## Description

The isotropic edge part of `insert_dislocation` shifted the atoms on the cut side by half the Burgers vector in two places: the atoms below the cut before the elastic displacement, and the atoms above it afterwards, judged by where they were by then. A plane of atoms just below the cut, which the displacement lifts across it, was shifted both ways, which left a fault from the core to the surface. atomsk behaves the same way. Above and below are now judged by where the atoms were before the displacement. Atoms that do not cross the plane are unchanged.

Evidence, from the dislocation analysis on the configurations of the atomsk comparison test:
- fcc edge along y: before, one line of family `other` with b = (-2.56, 1.48, 0); after, one `1/2<110>` with b = (-2.56, 0, 0);
- bcc edge along z: before, no line at all; after, one `1/2<111>`.

Tests:
- `test_atoms_lifted_across_the_cut_stay_below` fails without the fix.
- `test_as_atomsk` now allows exactly the atoms within 1 Å of the cut to differ from atomsk, by half the Burgers vector along it and nothing else.

Found by an audit of the dislocation analysis.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `7440163cb500`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.