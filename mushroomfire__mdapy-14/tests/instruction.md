# Issue

**Title:** Delete as few overlapping atoms as possible

## Description

## What this changes

`find_overlapping` selected the later atom of every pair that overlaps. With pairs 1-3 and 3-5 it deleted 3 and 5, although 5 is no longer too close once 3 is gone. The new rule: **delete as few atoms as a simple rule can, and leave no pair closer than the distance.**

- The atoms that overlap the fewest others are kept first, the lower index before the higher, and each atom kept deletes those it overlaps. The very fewest is NP-hard, so it is not sought.
- `build_polycrystal` uses the same rule. It did not delete too many before, but it kept atoms by index alone.
- An atom too close to its own periodic image is selected, as nothing else can part them.
- `keep_one=False` does not change: every atom that overlaps another.
- The comparison with OVITO is gone, as a purely geometric rule needs no reference, and so is the claim "as OVITO" in the documentation.
- `examples/two-phase-polycrystal.md` wrote the old rule by hand and now calls `find_overlapping`.

Atoms deleted, all rules leaving no pair too close:

| System | Before | By index | This rule |
|---|---|---|---|
| Random, 5000 atoms, d = 2.5 Å | 3216 | 2781 | 2581 |
| Polycrystal, 43 348 atoms, d = 1.79 Å | 1924 | 1894 | 1803 |
| Polycrystal, 43 348 atoms, d = 2.2 Å | 3416 | 3375 | 3094 |
| Polycrystal, 677 463 atoms, d = 1.79 Å | 20 784 | 20 670 | 19 664 |

It costs one sort: 358 ms for 677 463 atoms, against 310 ms by index.

The atom counts quoted on the pages changed slightly and are updated. The pictures of those pages are not regenerated, as the difference does not show.

## How the results were checked

Against all subsets of 12 atoms, over 60 random systems: no close pair is left, every atom deleted overlaps an atom kept, and the number deleted is within 10 % of the very fewest. Also chains, a star in every order of its atoms, groups of copies, and an atom too close to its own image, all with known answers.

## Checklist

- [x] `pytest` passes (3037 passed, 5 skipped locally); CI to confirm on Linux, macOS and Windows
- [x] `ruff check .` and `ruff format --check .` pass
- [x] Coverage is at least 95 % for each file changed (100 % for both)
- [x] `mkdocs build --strict` passes

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `84105a0d9ed1`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.