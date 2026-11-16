# Issue

**Title:** fix: Use Lattice::wrap_nearest_delta() for triclinic unit cells in NAMD

## Description

When the unit cell is triclinic, `colvarproxy_namd::position_distance()` switches to using a more expensive/robust function as suggested a while ago .

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `27eae2fa6e8f`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.