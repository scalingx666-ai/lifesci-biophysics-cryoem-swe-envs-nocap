# Issue

**Title:** Fix soft_sphere energy with r_off > 0 guard

## Description

soft_pair_force_factor treats a pair with r_off = dist - offset <= 0 as
non-interacting and returns a force factor of exactly 0 (its explicit
`if (r_off > 0.0)` guard). soft_pair_energy was missing that guard: inside
only the `dist < max_cutoff()` gate it unconditionally evaluated
a / pow(r_off, n). For an overlapping pair with offset > 0 (reachable via
initial overlap, other potentials/thermostat, or checkpoint restore) this
yields +Inf when dist == offset (r_off == 0) and NaN when dist < offset with
a non-integer exponent, while the corresponding force is 0 -- energy and force
silently disagree and the energy observable is poisoned.

Mirror the force kernel: wrap the energy computation in the same
`if (r_off > 0.0)` guard and return 0 otherwise, so the energy agrees with
the force everywhere. Returning 0 (rather than throwing) matches the sibling
force function, which already defines this region as non-interacting, and
keeps legitimately-reachable overlapping configurations from crashing the
energy analysis.

Adds a Boost unit test soft_sphere_test.cpp asserting that wherever the force
factor is 0 the energy is finite and equal to 0 (dist == offset => +Inf;
non-integer n with dist < offset => NaN on the unfixed code), while leaving
the genuinely-interacting region unchanged.

🤖 Generated with [Claude Code](https://claude.com/claude-code)

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `fdf846d9cb7d`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.