# Issue

**Title:** Linked cell list loop for steepest descent with shared memory parallelisation

## Description

To solve #5141, we implemented a link cell loop for steepest descent with sheared memory parallelisation because the 2D array Verlet list causes this issue.

Description of changes:
- `id_to_index` changed to a member variable of CellStructure
- To recognise weather steepest_descent is running, we added integ_switch as an argument to `cabana_short_range()`
- To impmenet link cell loop using Kokkos::parallel_for, we added the function `link_cell_kokkos()` in short_range_cabana.hpp and `link_cell_loop()` in CellStructure.hpp

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `a1b888b99475`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.