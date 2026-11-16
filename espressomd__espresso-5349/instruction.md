# Issue

**Title:** utils: guard Vector::normalized() against zero-length vectors

## Description

Vector::normalized() divided by the norm with no zero guard, so a zero
vector produced NaN components (0/0). This poisoned the Stoner-Wohlfarth
dipole update: when the external field is (anti)parallel to the easy
axis, vector_product(vector_product(e_h, e_k), e_h) is exactly the zero
vector, and normalized() turned it into NaN rot_axis, writing NaN into
the dipole moment and quaternion of the particle.

Fix normalized() to mirror the existing in-place normalize() guard:
return the vector unchanged when its length is zero instead of dividing
by zero. The result stays finite (the degenerate rot_axis is multiplied
by sin(phi)=0 in that configuration, so its value is irrelevant as long
as it is finite).

Adds a Boost.Test case (normalized_zero_vector) covering both the direct
zero-vector case and the exact degenerate Stoner-Wohlfarth expression.

🤖 Generated with [Claude Code](https://claude.com/claude-code)

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `5a0ebe689a99`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.