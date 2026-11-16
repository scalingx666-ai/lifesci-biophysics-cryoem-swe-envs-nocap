# Issue

**Title:** Sample all k vector directions in `StaticStructureFactorDirect`

## Description

## Description
All changes are in `reciprocal_isotropic` and its helpers in `StaticStructureFactorDirect.cc`, in both the 3D and 2D implementations. The grid indices are now signed ints, and it now samples half of the total grid, rather than an octant, because S(k) = S(-k).
Previously the indices were unsigned int starting at 0, so only the positive octant (quadrant in 2D) was sampled.

Other changes (some of these issues were hidden by having only positive vectors):

- `evaluate_quadratic_equation` now returns the integer range bracketing both + and - roots, or an empty range when the discriminant is negative. Previously it used only the + root, and when the discriminant was negative it cast a `NaN` to unsigned int, which is undefined behavior. The `k_max` range bounds the innermost loop, and the `k_min` range skips points inside `k_min`.

- The normalization in `get_prune_distance3D` and `get_prune_distance2D` is updated from an octant/quadrant to a half space/half plane (changed multiplicative prefactors) so `num_sampled_k_points` still gives about the requested number of k points.

The following changes fix issues with triclinic boxes.

- Sign of the linear term, `coef_b`  becomes `2 * dot(...)` instead of `-2 * dot(...)`. This only affects triclinic boxes, where the dot product is nonzero. For triclinic boxes, the dot product can be positive or negative, we should leave the sign as is before passing into the quadratic solver, which uses `-coef_b` when calculating the center. This was shifting the center, causing the quadratic solver to drop some possible candiddate indices. 

- Index bounds, `ceil(q_max * |a_i|)`, where `|a_i|` is the length of real-space box vector i, instead of `ceil(q_max / |b_i|)`, which can stop short in triclinic boxes. The separate bound on the innermost index is removed because the quadratic gives that range.


As mentioned in #1500, this PR was made with the assistance of Claude code.

## Motivation and Context
Previously, only the all-positive octant was being sampled when generating k vectors because of the use of unsigned int types and counting up during vector generation. This gave incorrect S(k) results for anisotropic structures

Resolves: #1500 

## How Has This Been Tested?
A unit test was added on a simple system where S(k) can easily be computed via numpy. `test_oriented_system_against_full_grid` places two particles in a box. For two particles, S(k) = 1 + cos(k·d), so the test compares freud's binned S(k) with a numpy average over every point of the reciprocal grid. It runs in a cubic box, a triclinic box and a 2D box. It fails on the current code in all three boxes and passes with this fix.

A couple  notes for maintainers:

1)  I think the test against dynasor unit test will always be skipped now if the CI workflow is installing the latest release of dynasor.  Its package level name is no longer `dsf` after their 2.0 release, and the test only runs if it can detect `dsf` . We could update the guard to test against latest versions of dynasor or replace it with the new test added by this PR. I haven't ran the test with the latest version of dynasor. 

2) There's a one-line change that seems to recover the ~3.5x slowdown from sampling the full grid. In `compute_F_k`, replacing `F_ki += std::exp(std::complex<float>(0, alpha));` with `F_ki += std::complex<float>(std::cos(alpha), std::sin(alpha));` made `compute()` about 3.6x faster in a quick local benchmark of 4,000 particles.

This works because the exponent's real part is always 0, so $\exp(i\alpha)$ is just $\cos\alpha + i\sin\alpha$.

The complete set of unit tests + the new one still pass with this change, and it doesn't need any changes to compiler flags. I'm happy to add it to this PR or open a separate one, whichever you prefer.

Also, I will make an effort to be active and respond, but also please feel free to make any edits you feel are necessary!

## Checklist:
- [x] I have reviewed the [**Contributor Guidelines**](https://github.com/glotzerlab/freud/blob/main/CONTRIBUTING.md).
- [x] I agree with the terms of the [**Freud Contributor Agreement**](https://github.com/glotzerlab/freud/blob/main/ContributorAgreement.md).
- [ ] My name is on the [list of contributors](https://github.com/glotzerlab/freud/blob/main/doc/source/reference/credits.rst) in the pull request source branch.
- [x] I have updated the [Change log](https://github.com/glotzerlab/freud/blob/main/ChangeLog.md).

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `ce9acf6732c8`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.