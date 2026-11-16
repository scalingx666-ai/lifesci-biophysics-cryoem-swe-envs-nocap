# Issue

**Title:** Two-set systems: rebuild only updated lists; build small first-set lists without touching all cells

## Description

This PR makes repeated updates of a small set queried against a large fixed set fast, in two commits.

## 1. Rebuild only the lists of the updated sets (regression fix)

The new cell list implementation (#186, not yet released) rebuilt both cell lists of a `ParticleSystem2` whenever either set was updated. Previous versions (0.10.5 and earlier) rebuilt only the lists of the sets whose positions changed.

`_update!(::ParticleSystem2, ::Type{CellList})` now rebuilds each list only if its positions are marked as updated. Changing the box or the cutoff still marks both sets, so both lists are rebuilt in that case, as before.

## 2. Small first-set lists built without touching all cells

The counting sort zeroes the counts, takes a prefix sum, and scans for nonempty cells. Each of these runs over the whole grid, so building even a 1-particle list costs time proportional to the number of cells.

The list of the first set of a two-set system (the "ref" list) is only iterated over. The pairwise computation reads its `cell_start` only at the list's own nonempty cells `c` and at `c+1`. When such a list has fewer than 1 particle per 64 cells, it is now built by `_build_small!`:

- Locate the particles, as before.
- Sort them by (cell, index), packed into one integer key. The sort is `QuickSort` on a scratch buffer kept in the `CellList`, so it doesn't allocate.
- Write `particles`/`shifts` in that order, and set `cell_start` only at the nonempty cells and the cells right after them. All other entries are never read and are left as they were.

The resulting `particles`, `shifts` and `nonempty_cells` are identical to those of the counting sort. Self lists, second-set lists, and large first-set lists still use the counting sort. The speedup applies when the small, frequently updated set is the first one (`xpositions`).

Build time, small build vs. counting sort, cutoff 1:

| cells | particles | counting sort | small build |
|---|---|---|---|
| 10⁶ | 4 | 730 µs | 0.05 µs |
| 10⁶ | 1,000 | 746 µs | 15 µs |
| 10⁶ | 10,000 | 873 µs | 364 µs |
| 10⁵ | 1,000 | 77 µs | 15 µs |
| 10⁵ | 3,000 | 100 µs | 70 µs |
| 10⁵ | 10,000 | 184 µs | 347 µs (counting sort faster) |

The two methods break even at about 20–35 cells per particle. The 64-cells-per-particle threshold keeps the small build on the faster side.

## Impact

Packmol.jl places each molecule by trying up to 20 random positions, and checks each trial molecule (1–17 atoms) against a fixed 490k-atom virus capsid, using a grid of 487k cells. Full system: 2.77M atoms, 32 threads.

| | `main` | regression fix only | + small lists |
|---|---|---|---|
| one trial (update + pairwise) | 7.9 ms | 0.35 ms | 0.8 µs |
| placement phase | 43 min | 4 min 20 s | < 1 s |
| whole startup (constraint fitting, placement, fitting against the fixed structure) | ~56 min | 5 min 7 s | 22 s |

## Tests

- "ParticleSystem2: only the updated set is rebuilt": changes the second set's storage without marking it as updated, then checks that its list isn't rebuilt when only the first set is updated. It also compares results with freshly built systems. The test fails without commit 1.
- "small reference lists": for random small sets in orthorhombic and triclinic cells, including positions outside the cell that need wrapping, the small build gives the same `particles`, `shifts`, `nonempty_cells`, and `cell_start` at every entry that is read, as the counting sort. Two-set `pairwise!` with the small set first also matches the computation with the sets swapped, in periodic (orthorhombic, triclinic) and non-periodic systems, over repeated updates.

Changelog: one Info entry under 0.10.7-DEV.

All tests pass locally (2429).

🤖 Generated with [Claude Code](https://claude.com/claude-code)

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `8faaf3333a47`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.