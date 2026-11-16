# Issue

**Title:** fix(flash): auto declines a free-space structure too spread out for the cell grid

## Description

`so3lr eval --engine auto` could choose flash for a structure with no box and then fail during the evaluation with `cell grid ... has 2^28 cells or more`. This happens with binding benchmarks, where one monomer is translated far away: the box flash lays out grows with the atoms' span.

- `check.grid_problem` compares the cell count with `MAX_CELLS`, which moves here from `engine.py`. The engine's `_layout` raises its reason, so the check exists only once.
- `structure_problem` checks the grid of the free-space box, and of a periodic box. `--engine auto` then falls back to the neighbour-list potential and logs why; `--engine flash` still raises.
- `frames_problem` checks every free-space structure that spans more than the ones before it, not only the first.
- A free-space structure with no atoms, or with positions that are not finite, is declined instead of making the check raise.

Tests: in `tests/test_flash.py`, engine-choice cases for spread-out, empty and non-finite structures and for large boxes, and a GPU test in which `so3lr eval` falls back for a dimer with one monomer moved away.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `bdbfe309c412`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.