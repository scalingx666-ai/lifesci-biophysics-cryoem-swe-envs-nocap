# Issue

**Title:** Fixed error in doctests

## Description

Numpy 1.14.0 has a different output format
than 1.13.0. The doctests and the required
version is updated accordingly.

**********************************************************************
File "testsuite.rst", line 36, in default
Failed example:
    print(low[0:3])
Expected:
    [ 0.  1.  2.]
Got:
    [0. 1. 2.]
**********************************************************************
File "testsuite.rst", line 38, in default
Failed example:
    print(avg[0:3])
Expected:
    [ 0.01  0.02  0.03]
Got:
    [0.01 0.02 0.03]

Similarly in guessing_radii.rst ...

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `4108c7aaee8e`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.