# Issue

**Title:** shapes: fix SpheroCylinder leaving m_direction uninitialized

## Description

The `SpheroCylinder` default constructor did not initialize `m_direction`,
while sibling shapes `Cylinder` and `Torus` initialize it to `1.0`.
`calculate_dist` multiplied by `m_direction`, so a `SpheroCylinder` created
without the `direction` keyword yielded garbage-signed constraint distances
and silently wrong constraint forces.

**Fix:** add `m_direction(1.0)` to the constructor initialization list in
`src/shapes/include/shapes/SpheroCylinder.hpp`.

**Regression test:** `src/shapes/unit_tests/SpheroCylinder_test.cpp` (Boost.Test).

Fixes bug #8 of the adversarial core bug sweep.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `ffa19d338227`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.