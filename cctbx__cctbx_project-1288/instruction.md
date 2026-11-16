# Issue

**Title:** scitbx.math: Add fibonacci_sphere_points and saff_kuijlaars_sphere_points

## Description

## Problem
`scitbx.math` has no function that spreads an arbitrary number of points
over the sphere; `icosahedron` gives only 12, 80, 320, ... points.
`maptbx.spherical_variance_around_point` builds a Saff-Kuijlaars spiral
inline in its constructor.

## Changes
- `scitbx/math/__init__.py`: `fibonacci_sphere_points(n)` (golden-angle
  spiral) and `saff_kuijlaars_sphere_points(n)`, each returning n unit
  vectors about the y axis.
- `cctbx/maptbx/__init__.py`: `spherical_variance_around_point` takes its
  points from `saff_kuijlaars_sphere_points`; the points are bit-identical
  to those of the inline spiral.

## Tests
- `scitbx/math/tests/tst_math.py` (`exercise_sphere_points`): point count,
  unit length, the height spacing of both spirals, the golden-angle step
  and the Saff-Kuijlaars poles; passes on macOS arm64 and Linux.
- `cctbx/maptbx/boost_python/tst_maptbx.py` and the `mmtbx/ions` tests
  pass on macOS arm64 and Linux.
- Full Phenix suite on Linux (t96): no new failures compared to master.

## Coding agent
| Field | Value |
|-------|-------|
| Model and version | Claude Opus 5.5 (`claude-opus-5-5`) |
| Harness and version | Claude Code 2.1.311 |

## Checklist
- [x] Searched open and closed PRs and issues for prior work: #1285 (its
      review asks for the sphere helpers in `scitbx`)
- [x] New or changed tests are registered in the module's `run_tests.py`
- [x] Docstrings and PHIL `.help` strings cover the new or changed behavior
- [x] No new external dependency, or it was agreed with the developers
- [x] No imports of modules outside this repository; `chem_data` and
      `phenix_regression` are used only for data, with a skip when absent
- [x] Interface or breaking changes are called out under Changes
- [x] A human has reviewed the complete diff

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `a3155f43569f`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.