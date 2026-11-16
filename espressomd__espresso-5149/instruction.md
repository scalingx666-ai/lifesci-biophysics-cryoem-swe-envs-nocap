# Issue

**Title:** Change coulomb_energy_kernel interface from AOS to SOA

## Description

The computational rate with electrostatic interaction is improved by 10% without shared memory parallelization. With shared memory parallelization, there is no improvement.

Description of changes:
- Change API of Coulomb energy kernels
- Modify `rebuild_local_properties` to reallocate local properties during a rebuild instead of recreating them
- Remove code duplication in non-bonded kernels

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `43d01df00353`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.