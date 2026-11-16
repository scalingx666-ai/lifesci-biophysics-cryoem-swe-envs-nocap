# Issue

**Title:** plot.warren_cowley draws the parameters as a matrix

## Description

## What this changes

`plot.warren_cowley(result, *, ax=None, shell=None, vmin=-2.0, vmax=1.0, cmap="GnBu")` draws the Warren–Cowley parameters as a matrix of colors with their values written in, the plot MDAPY 1.x had and many papers use. The defaults, the colors and the layout are those of 1.x: the central element along x, its neighbor along y, the first element at the bottom left.

Three differences from 1.x:

- The labels fit the data. In 1.x the rows held the central element while the axis of rows said "Neighboring element". With a cutoff the matrix is symmetric and it did not show; with a number of neighbors it can be asymmetric.
- Values on dark cells are written in white, and a parameter that cannot be told (an element whose atoms have no neighbors) shows as –.
- For parameters of several shells, `shell` chooses the one to draw.

## How the results were checked

- Cu3Au in the L1_2 order with 12 neighbors against the analytic values: Cu–Cu 1/9, Cu–Au -1/3, Au–Au 1, each written in its cell.
- An asymmetric matrix puts each pair at its place: `alpha[a, b]` at x = a, y = b.
- The CsCl order by shells: the second shell of CuZn, and the errors for a shell that is missing or does not exist.

## Checklist

- [x] `pytest` passes for the plots and the examples of the pages; CI to confirm on Linux, macOS and Windows
- [x] `ruff check .` and `ruff format --check .` pass
- [x] Coverage is at least 95 % for each file changed
- [x] `mkdocs build --strict` passes

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `da38e35da90f`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.