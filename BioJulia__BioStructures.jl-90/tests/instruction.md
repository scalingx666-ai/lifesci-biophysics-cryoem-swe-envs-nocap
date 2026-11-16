# Issue

**Title:** Fix DomainError and silent 0.0 in bondangle/dihedralangle on degenerate geometry

## Description

Two small correctness fixes in the vector-form methods of `src/spatial.jl`.

1. `bondangle` can throw `DomainError` on legal geometry.

It computes `acos(dot / (norm * norm))` with no clamp. For near-parallel or
near-antiparallel bonds, floating point rounding can push the ratio just outside
`[-1, 1]` (for example `-1.0000000000000002`), and `acos` then throws. This is a
defensive fix: clamp the argument to `[-1, 1]` before `acos`. A genuinely coincident
atom still gives `0/0 = NaN`, which is left as the correct undefined result.

2. `dihedralangle` returns `0.0` for degenerate input.

For four collinear points, or a coincident pair, both `atan` arguments vanish and
`atan(0, 0)` is `0.0`. That is a valid looking value for an undefined angle, and it
flows into `phiangles`/`psiangles`/`omegaangles`/`chiangles`. The fix returns `NaN`
when both arguments are zero, matching the convention those same functions already
use for undefined angles. A genuine planar dihedral (0 or pi) is unaffected, since
only the degenerate case has both arguments zero. Caching `cross(vec_b, vec_c)`,
previously computed twice, keeps non-degenerate results identical and drops one
allocation.

Tests were added to the existing Spatial testset: a near-linear triple no longer
throws and returns approximately pi, and collinear and coincident-pair dihedrals
return NaN. Each assertion was confirmed to fail on the current code and pass after
the fix. The full test suite stays green, and a quick benchmark shows bondangle
unchanged and dihedralangle slightly faster with two fewer allocations.

Happy to add a NEWS.md entry if you would like one.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `bd1c385c9c3c`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.