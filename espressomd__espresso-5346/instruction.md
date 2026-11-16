# Issue

**Title:** shapes: fix Torus::calculate_dist producing NaN for particles on the torus axis

## Description

When a particle lies on the torus symmetry axis (`r == 0`), `r_vec` is the zero
vector and `r_vec * m_rad / r` computes `0 * inf = NaN`, propagating NaN into
the constraint force direction.  Sibling axial shapes (`Cylinder`, `SimplePore`,
`SpheroCylinder`) all branch on `r == 0` to avoid the division.

**Fix:** add an `e_r_axis` member computed via Gram-Schmidt; replace the bare
`r_vec * m_rad / r` with `e_r * m_rad` where
`e_r = (r == 0) ? e_r_axis : r_vec / r`
(`src/shapes/include/shapes/Torus.hpp`, `src/shapes/src/Torus.cpp`).

**Regression test:** `src/shapes/unit_tests/Torus_test.cpp` (Boost.Test).

Fixes bug #14 of the adversarial core bug sweep.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `426841834061`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.