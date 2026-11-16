# Issue

**Title:** Add Langevin simulator for more general splittings

## Description

Add support for more general splitting schemes for Langevin dynamics. 
# Changes
- [x] Add `LangevinSplitting` type and `simulate!` method to simulate.jl
- [x] Add docstring describing the simulator
- [x] Write test comparing the current Langevin and LangevinSplitting with identical RNG, in the case of a BAOA splitting.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `2e35605921b8`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.