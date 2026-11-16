# Issue

**Title:** Fix rescaling of non-cubic boxes

## Description

`change_volume_and_rescale_particles("xyz")` computed a single scalar scale
from the x-axis (`length * length_inv()[0]`) and applied it to all three
particle coordinates, silently corrupting y/z positions on any non-cubic
box.  The cubic-box assert was compiled out under NDEBUG.

**Fix:** for `coord==3`, capture old per-axis lengths, apply shrinking axes
before the box resize and growing axes after, each with its own
`length/old_lengths[axis]` scale factor
(`src/script_interface/system/System.cpp`).  Also removes the now-vacuous
cubic-box assert.

**Regression test:** `test_isotropic_rescale_noncubic_box` added to `Test`
in `testsuite/python/system.py`.

Fixes bug #16 of the adversarial bug sweep.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `cd7547c43b2d`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.