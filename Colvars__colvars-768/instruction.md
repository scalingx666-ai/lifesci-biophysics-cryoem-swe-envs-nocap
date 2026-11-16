# Issue

**Title:** fix: clamp the input values of asin and acos in case of fast math on aarch64

## Description

This PR clamps the input values of `asin` and `acos` in case of `__FAST_MATH__` and `__aarch64__`, which fixes `NaNs in tests `000_distancedir-fitgroup_harmonic-ddir-fixed` and `000_distancedir-fitgroup_harmonic-ddir-fixed-fit-forces`.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `fff3b831da2a`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.