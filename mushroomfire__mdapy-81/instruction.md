# Issue

**Title:** DislocationAnalysis: the atoms of the cores

## Description

Builds on #79 (and #78, #77), whose commits it contains until those merge.

`DislocationAnalysis(..., core_atoms=True)` marks the atoms of the core of each line, as OVITO's option `mark_dislocation_core_atoms` does. `DislocationResult.core_atoms` is the (N,) int64 index into `lines` of the line whose core holds each atom, −1 for none, and it is also the column `dxa_core_atoms`.

**Rule.** OVITO's manual says the core is what lies on or inside the Burgers circuits, whatever the structure of the atoms. In MDAPY's data, the core of a line is the bad crystal its circuits enclose:

- **Seeds**: the tetrahedra that are neither good nor empty space (r² < alpha) behind the triangles the line's circuits took for good.
- **Flood**: from the seeds through further bad tetrahedra.
- **Sharing**: where this meets bad crystal reached from the defect mesh, each tetrahedron goes to the source it is the fewest steps from. A line beats the defect mesh, and the first line beats the others.
- **Core atoms**: the corners of a line's tetrahedra; an atom in two cores goes to the first line.

The work is serial and deterministic.

**Against OVITO 3.17.1**, atom by atom (pip OVITO marks core atoms). In the table, *mesh* means the atoms where the two tessellations differ by ties, which were already there:

| Sample | OVITO | MDAPY | both |
|---|---:|---:|---:|
| perfect fcc loop | 128 | 128 | 128 |
| Shockley loop | 110 | 110 | 110 |
| bcc loop | 47 | 47 | 47 |
| crossing loops with junctions | 224 | 223 | 222 (mesh) |
| loop through bad crystal of 3 / 5 / 7 Å | 142 / 203 / 299 | 142 / 202 / 299 | all but 1 (mesh) |
| periodic lines 7–11 Å long (bcc, fcc, diamond, hcp, lonsdaleite) | | | the same atoms, of the same line |

Where they differ:

- **Longer periodic lines** (from some 9 Å in Cu, 12 Å in Fe). OVITO marks 3 to 15 good-crystal atoms next to the core at one place of each line, and in long boxes it leaves stretches unmarked: 37 Å of a partial in a 53 Å box. I checked the bcc screw quadrupole independently: OVITO's 5 extra atoms are all bcc, 2.8–3.9 Å from the line, while every MDAPY core atom is within 2.5 Å. MDAPY marks the same atoms in every period; a test checks a ×3 replicated box against the tiled result.
- **Free ends**: lines ending on free surfaces differ by a few atoms, because the line ends lie Å apart (3 of 44).
- **Lines ending inside bad crystal** their circuits do not pass around: the share-out differs (9 Å blob: 103 against 80 of 146 atoms).
- **A 6 Å void**: 4 more atoms near the empty-space threshold.

The DXA page explains each of these in "Where the results differ from OVITO".

Tests:

- `tests/analysis/test_dislocations.py`: invariances (threads, translation, replication), the option off leaving the result unchanged with `core_atoms is None`, the column, and junctions.
- `tests/analysis/test_dislocations_ovito.py`: the comparisons above, exact up to three mesh atoms, with bounded checks on the long periodic lines.
- The C++ thread checks run with the option on.

120 DXA tests and the docs/API tests pass locally; `_dislocations.py` is at 100 % coverage. The page gets a section "Core atoms" with a picture, a definition paragraph, glossary entries and a release-notes line.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `e7eb0ac02700`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.