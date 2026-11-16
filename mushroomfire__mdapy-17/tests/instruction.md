# Issue

**Title:** build_crystal runs on all threads

## Description

Follows #16 and is based on it; merge that one first.

## What this changes

`build_crystal` runs on all threads and gives the same atoms in the same order. It was serial NumPy, and for a large crystal it took longer than `build_polycrystal`.

- A new kernel writes the positions and the kinds of the atoms directly, each thread at its own place in the arrays. No temporary arrays of cells and atoms are made, and no BLAS is called.
- A crystal of one element takes its element column as a constant, which is 50 times faster than gathering it.
- A `repeat` so large that the number of atoms would overflow raises `InvalidParameter`. A crystal below 4096 rows of atoms stays on the calling thread.
- [Building structures](docs/performance/builders.md) is measured again.

Building alone, 9,841,500 atoms of fcc copper:

| Threads | Before (s) | After (s) | Faster |
|---:|---:|---:|---:|
| 1 | 0.251 | 0.044 | 5.7 |
| 10 | 0.254 | 0.017 | 15 |

The memory of the build at that size goes from about 750 MB to about 300 MB.

Building and writing, 10 threads, against Atomsk: the single crystal is now 16 to 23 times faster than Atomsk (0.46 s against 7.5 s at 9,841,500 atoms). It was 10 to 25 times faster before this change on the same machine (0.67 s).

## How the results were checked

- Against the previous code: 1058 cases (4.7 million atoms) of all structures, with one element and with several, along chosen directions, with `repeat` long along each vector. None differ in the order of the atoms, the elements, the box or the schema. The largest difference of a position is 5.7e-14 Å, rounding between BLAS and the plain formula.
- New tests: the atoms of 12 structures and orientations against plain nested loops, at 6 values of `repeat`; large crystals, also long along each vector, the same on 1 and 4 threads, to the last bit, and against those loops; what the kernel refuses.
- ThreadSanitizer on the C++ checks passes, also against plain loops. g++-15 with the warnings of CI is clean on the changed C++ files. The tests of the builders pass on Python 3.11 with NumPy 2.3.1 and polars 2.0.0.
- AddressSanitizer and Windows were not run locally; CI covers them.

## Checklist

- [x] `pytest` passes (3178 passed, 6 skipped locally); CI to confirm on Linux, macOS and Windows
- [x] `ruff check .` and `ruff format --check .` pass
- [x] Coverage is at least 95 % for each file changed (`_crystal.py` 100 %)
- [x] `mkdocs build --strict` passes

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `0ae57fba628f`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.