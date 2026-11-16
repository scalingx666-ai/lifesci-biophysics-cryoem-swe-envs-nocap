# Issue

**Title:** PolyhedralTemplateMatching: the nearest neighbors found as needed

## Description

Without a neighbor list, `PolyhedralTemplateMatching` now finds the 18 nearest neighbors of each atom in a k-d tree as it needs them, rather than building a list first, as the nearest-neighbor kernels of CNA, CSP and Ackland–Jones already do.

- **One pass where one shell is enough.** For fcc, hcp, bcc, ico and sc, each atom's neighbors are found once, ordered by their Voronoi faces and matched at once. Diamond and graphene still need two passes, because their shells take the neighbors of neighbors in their order. A given list uses the same passes.
- **Same results.** The neighbors come from the same search as `NeighborList.build(num_neighbors=18)`, so the results are the same bit for bit. Tests compare every output, both pass layouts, for shuffled hcp, diamond, graphene and L1₂, a 4-atom periodic box and a cluster; the C++ thread checks (also under TSan) do the same across thread counts.
- **Progress** counts once over the two passes; it started again at the second.

Measured on an Apple M5 (10 cores), memory beyond the system:

| Case | Before | After | Time before (list) | Time after |
|---|---:|---:|---:|---:|
| fcc Cu, 2 048 000 atoms, fcc/hcp/bcc | 752 MB | 293 MB | 7.9 s | 8.0 s |
| diamond Si, 1 000 000 atoms, both diamonds | 376 MB | 139 MB | 4.3 s | 6.3 s |

For diamond and graphene the neighbors of neighbors are searched again, which is slower; the page says that a list of the 18 nearest is faster for those, at the cost of memory.

All 3839 tests pass locally; `_ptm.py` coverage is 100 %.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `5e4a0aae9a28`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.