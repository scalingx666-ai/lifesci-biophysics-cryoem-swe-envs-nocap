# Issue

**Title:** remove deprecated oldest-supported-numpy

## Description

I was a bit surprised to see this dependency still in the build-system part as it also defines `numpy>2`.

It is officially [deprecated](https://github.com/scipy/oldest-supported-numpy?tab=readme-ov-file#deprecation-notice-for-numpy-20) and the backwards compatibility issue has been solved by numpy since version `1.25` (before that oldest-supported-numpy was needed for build stability).

This is what the [current numpy docs](https://numpy.org/devdocs/dev/depending_on_numpy.html#build-time-dependency) say:
> By default, NumPy will expose an API that is backwards compatible with the oldest NumPy version that supports the currently oldest compatible Python version. NumPy 1.25.0 supports Python 3.9 and higher and NumPy 1.19 is the first version to support Python 3.9. Thus, we guarantee that, when using defaults, NumPy 1.25 will expose a C-API compatible with NumPy 1.19. (the exact version is set within NumPy-internal header files).

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `ccd0c874361e`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.