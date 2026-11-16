# Issue

**Title:** Gzip files are written on all threads, and LAMMPS data files as fast as dumps

## Description

## What this changes

Gzipped files are written on all threads, and LAMMPS data files are read and written about as fast as dumps. The text and the values do not change. The performance pages are measured again, up to nearly 10 million atoms, with gzipped cases.

Gzip:

- Blocks of 1 MiB are compressed on all threads and joined into one gzip stream, as pigz does. Each block looks back into the one before it, so the file is as small as with one thread, and its bytes do not depend on the number of threads.
- It is one stream on purpose: OVITO stops at the end of the first stream of a file, which many streams one after another would break. OVITO, ASE, Python and `gzip -dc` read these files.
- The basics page now recommends `.gz`. The other formats still compress on one thread.

LAMMPS data files:

- IDs are checked for repeats in one pass, without sorting: a comparison when they are in order, a table when they are close together. Sorting remains for IDs far apart.
- Atoms are found by ID through a table, and only when the file has velocities or topology.
- What depends on the type (the type, the mass, the element, the label) is found once for each type. Labels no longer make a Python string for every atom.
- Writing groups the types and elements with polars for many atoms, and counts them for few, as grouping wakes every thread and took longer than the rest of a small file.

## Results

Reading and writing, 9,841,500 atoms, 40 threads on an Intel Xeon Gold 6148, the shortest of three. Times in seconds.

| Case | MDAPY before | MDAPY now | OVITO | ASE |
|---|---:|---:|---:|---:|
| Read a data file | 0.623 | 0.145 | 3.46 | 44.7 |
| Write a data file | 0.519 | 0.347 | 5.91 | 34.1 |
| Read a dump | | 0.122 | 2.33 | 14.0 |
| Write a dump | | 0.334 | 5.77 | |
| Read a gzipped data file | 4.127 | 3.525 | 6.68 | 48.6 |
| Write a gzipped data file | | 2.023 | 69.9 | 373.5 |

Small files gain too: at 4,000 atoms a data file takes 1.2 ms instead of 1.4 ms to read and 1.1 ms instead of 2.7 ms to write.

Gzip against the single stream of one thread, a dump of 2,048,000 atoms with every digit on an Apple M5 with 10 threads:

| Threads | Write (s) | Size (MB) |
|---:|---:|---:|
| 1 | 4.5 | 57.1 |
| 10 | 0.73 | 57.1 |

[Reading and writing files](docs/performance/files.md) and [Building structures](docs/performance/builders.md) now run on a Xeon Gold 6148 node, as each says at its top. `comparison.md` stays on a Xeon Gold 6248 node.

## How the results were checked

- Against OVITO and ASE on the new code, at 2,048,000 atoms: the positions each reads are those MDAPY reads, and MDAPY reads back what each writes, for plain and gzipped data files.
- Gzip tests: one stream, checked with `zlib`; the same bytes on 1, 2 and 4 threads; as small as one thread within 1 %; every size around a block; the `gzip` program; a file that cannot be written; an unfinished file removed.
- Data tests: IDs in order, close together, far apart and not positive, with velocities and bonds out of order; repeated IDs naming their line; IDs no atom has; one type and several; labels; many atoms and few when writing.
- `tests/io` passes on Python 3.11 with NumPy 2.3.1 and polars 2.0.0.

## Checklist

- [x] `pytest` passes (3439 passed, 7 skipped locally, before the last two small commits on writing data; `tests/io` after them, 833 passed); CI to confirm on Linux, macOS and Windows
- [x] `ruff check .` and `ruff format --check .` pass
- [x] Coverage is at least 95 % for each file changed (`_files.py` 98.7 %, `_lammps_data.py` and `_lammps_dump.py` 100 %)
- [x] `mkdocs build --strict` passes

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `00ef09dabbea`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.