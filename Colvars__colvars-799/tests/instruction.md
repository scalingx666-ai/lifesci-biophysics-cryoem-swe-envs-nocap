# Issue

**Title:** Preparation work for Weighted Poisson integration

## Description

This is making substantial changes to the Poisson integration code, and this is creating many conflicts with recent changes in the grid code.

Incidentally, colvargrid.cpp is way too long, largely because of the complex `integrate_potential` class. I'm thinking it would be better to split it off into its own file first in master, and then squash and merge this PR so that there is less interference with the colvar_grid classes.

What do you think @giacomofiorin ?

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `4d7d899ad9b8`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.