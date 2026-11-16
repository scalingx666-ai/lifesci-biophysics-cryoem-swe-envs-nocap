# Issue

**Title:** Option to set atomic masses in walkers

## Description

Added `masses` argument to the `Walker`-class. If a `float` is provided, the atomic masses from ASE are scaled by this value. Individual atomic masses can be set by providing a `list` containing a value for each atom in the system. It doesn't look like units can be provided in i-Pi for atomic masses, therefore atomic mass units are expected and these are converted to internal i-Pi units automatically.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `e3b35332fc82`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.