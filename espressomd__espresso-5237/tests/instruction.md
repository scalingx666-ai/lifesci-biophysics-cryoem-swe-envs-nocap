# Issue

**Title:** Cabana: base max neighbor estimation on local density

## Description

For the Cabana based Verlet list, we need to know the maximum number of neighbors any particle has before the list creation is begun.
The existing code made an assumption about the local density (it considered the number of particles but not the local box size). Hence, the code failed for high densities, e.g., for a dpd fluid, and fell back to linked cell.

This PR bases the heuristics on the local partilce denisty (local here means: for the mpi rank)

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `27f23106886e`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.