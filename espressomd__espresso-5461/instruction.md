# Issue

**Title:** Balance the red/black cell pairing at periodic boundaries

## Description

Part of performance/scalability wave.
Improves parallel efficiency on openmp and mpi by reducing load imbalance between the ranks/threads.

Each cell pair is computed by one of its two cells: the neighbor is "red" for the cell that owns the pair. The owner was decided by comparing the folded linear indices of the two cells in the global cell grid. Across a periodic boundary the folded index of the neighbor image is large on the lower side and small on the upper side, so the cells of the first layer own the pairs across the boundary in addition to their own half shell: on a fully periodic grid, a cell in the first z-layer that is not on an x/y boundary has 22 red neighbors instead of 13 (the corner cell 26), a cell in the last z-layer 4. The Verlet list construction and the pair loop then have more than the average work in the first slab of cells (one OpenMP thread of the static cell partition) and on the MPI ranks at the lower domain boundaries.

Decide by the offset to the neighbor image instead: the pair belongs to the cell from which the offset is positive in lexicographic order (z most significant). On a fully periodic global cell grid with at least three cells per direction every cell then owns exactly 13 of its 26 neighbors, on one rank (where ghost cells are replaced by the periodic images) and on several ranks alike; both ranks of a pair decide from global cell coordinates, so no communication is needed. Where a neighbor cell is reached by more than one image (less than three cells in a periodic direction) or with a fully connected boundary, both cells of the pair fall back to the folded index comparison, as before: both cells of a pair see the same number of images, so they always apply the same rule. Non-periodic walls and these fallback cases keep an uneven distribution. In particular, Lees-Edwards systems with a fully connected boundary keep the old rule for all pairs; applying the offset rule to the pairs that are not fully connected is left for a follow-up.

The set of pairs is unchanged; only the cell that computes a pair changes, so forces agree to round-off.

Tests: a new unit test (RegularDecomposition_test, 1-4 MPI ranks) checks 13 red neighbors per cell and that every pair is owned exactly once on a periodic 4x4x4 (6 cells per direction for 3 ranks) global cell grid; it fails with the old rule. The new test case in regular_decomposition.py checks the pair loop and the forces against the N-square cell system on boxes with one to four cells per direction, periodic and non-periodic; boxes whose local box is smaller than the interaction range on the node grid in use are skipped.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `0636424a2aef`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.