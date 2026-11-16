# Issue

**Title:** build_polycrystal places only the atoms of each grain, on all threads

## Description

## What this changes

`build_polycrystal` gives the same atoms in the same order, much faster and with much less memory.

- A new kernel places only the atoms of each grain. It clips each row of copies of the unit by the planes of the Voronoi cell, so no atoms are generated and then thrown away, and it runs on all threads. Before, about 80 % of the time was serial NumPy on about 11 candidates per atom kept.
- Atoms too close are searched for only near the faces of the grains when `min_distance` is below the nearest distance of the unit. Molecules and columnar polycrystals still search all atoms.
- `Box.wrap` runs on all threads.
- [Building structures](docs/performance/builders.md) is measured again, up to nearly 10 million atoms. This page now runs on an Apple M5 with 10 threads, as it says at its top; the other performance pages stay on a cluster node.
- The picture of the comparison has its legend beside the bars, which it covered at the new sizes, and the table and the picture of `benchmarks/comparison.py` say MDAPY.

`__version__` stays 2.0.0a1 until the release, so the benchmark recorded that version. The label in the table and the picture was set to 2.0.0a2 by hand, and the page says "in development".

Before and after, with 10 threads on the same machine, building only:

| Case | Atoms | Before (s) | After (s) | Faster |
|---|---:|---:|---:|---:|
| 2 million, default distance, 20 grains | 1,968,149 | 1.988 | 0.057 | 35 |
| 2 million, `min_distance` 0.01 Å, 20 grains | 1,999,887 | 1.904 | 0.017 | 109 |
| 2 million, default distance, 2000 grains | 1,858,548 | 4.036 | 0.152 | 26 |
| 2 million, `min_distance` 2.7 Å, above the nearest distance | 467,069 | 2.524 | 0.681 | 4 |
| 250 000, columnar | 131,910 | 0.176 | 0.011 | 17 |
| Ice, 100 Å box, 8 grains | 89,412 | 0.149 | 0.024 | 6 |
| Ice, 300 Å box, 30 grains | 2,468,877 | 3.650 | 0.576 | 6 |

With `min_distance` 0.01 Å at 2 million atoms, the peak memory of the process goes from 1043 MB to 182 MB.

## How the results were checked

- Against the previous code, on this code: 67 chosen cases and 400 random ones (1.42 million atoms), none differ. Compared are the order of the atoms, grain, element, molecule, image, topology and schema, and positions within 1e-9 Å.
- New tests: the atoms of each grain against a NumPy search by hand, in 14 cases (triclinic boxes, atoms on faces, one seed, seeds outside the box, hcp, a primitive cell, columnar along each axis, seeds not in one plane, water); the atoms removed against a search of all atoms; the same result for any number of threads; `Box.wrap` against its formula.
- The benchmark checks each build against Atomsk before timing it, as the page describes, and all 10 agree.
- ThreadSanitizer on the C++ checks passes, g++-15 with the warnings of CI is clean on the changed C++ files, and the changed test files pass on Python 3.11 with NumPy 2.3.1 and polars 2.0.0.
- AddressSanitizer and Windows were not run locally; CI covers them.

## Checklist

- [x] `pytest` passes (3089 passed, 6 skipped locally); CI to confirm on Linux, macOS and Windows
- [x] `ruff check .` and `ruff format --check .` pass
- [x] Coverage is at least 95 % for each file changed (`_polycrystal.py` 99.7 %, `box.py` 100 %)
- [x] `mkdocs build --strict` passes

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `7c5d96914e7c`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.