# Issue

**Title:** melting_point: the answer when two stretches agree

## Description

Item 4 of the review of the physics. It was agreed with the maintainer, and the results are here for review before merging.

**The change.** A run is the answer only when the mean temperatures of its last two stretches of coexistence agree, within `tolerance` or twice their combined statistical error, in addition to the drift within the last stretch. A run therefore has at least two stretches.
- Before, one stretch could pass while the temperature still rose. The review found 1292 ± 0.6 and 1273 ± 1.7 K from two guesses at 4312 atoms.
- `error` stays the statistical error of one run. The docstring and the page say it is smaller than the spread of runs from other guesses or seeds.

**The copper table of the page**, rerun on Triton with the same scripts, potential and GPUs as before: Mishin EAM in LAMMPS with KOKKOS on V100 GPUs, guesses 1000, 1327 and 1700 K.

| Atoms, coexistence | Before (K) | Now (K) | Spread before → now |
|---|---|---|---|
| 4000, 100 ps | 1316, 1325, 1326 | 1328, 1333, 1326 | 10 → 7 |
| 4000, 500 ps | 1326, 1318, 1328 | 1321, 1322, 1321 | 10 → 1 |
| 16000, 100 ps | 1322, 1319, 1336 | 1321, 1322, 1320 | 17 → 2 |
| 16000, 500 ps | 1327, 1328, 1324 | 1325, 1323, 1325 | 4 → 2 |

The free energies of 16384 atoms cross at 1326 K. Each search took 5 to 45 minutes on one V100.

**The example run** of the page (16000 atoms, 500 ps, from 1000 K) now gives 1323 ± 0.7 K.
- The runs from 1000 and 1150 K froze, as before.
- The run from 1322 K needed five stretches: 1325.4, 1321.3, 1320.8, 1320.8 and 1322.6 K. The fourth agreed with the third but still drifted within itself; the fifth agreed with the fourth.
- The picture was redrawn from that run.

**Tests**: `_steady` on two stretches that agree or not, with and without noise. A steady run has at least two stretches. The searches on argon from both sides still meet. The 18 melting tests pass.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `590a81bb1956`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.