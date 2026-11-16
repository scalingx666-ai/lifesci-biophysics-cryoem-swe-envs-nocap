# Issue

**Title:** Remove old compint

## Description

This pull request updates the version of the package to 1.5.0 and refactors several integration routines for improved readability and maintainability. The most significant changes involve streamlining the thermodynamic integration code, removing unused parameters related to composition scaling, and enhancing code formatting and consistency throughout the `calphy/integrators.py` module.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `91a8755600e4`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.