# Issue

**Title:** Restore exclusions setter in ParticleHandle update method

## Description

Trying to update the exclusions property via the `update` method of a particle failed silently as exclusions are not part of the list of `AutoParameters` added to `ParticleHandle` objects, and thus not affected by calls to `do_set_parameter`.

- Added logic for setting exclusions in code for _update_params_ in `do_call_method` of [`ParticleHandle`](https://github.com/espressomd/espresso/blob/python/src/script_interface/particle_data/ParticleHandle.cpp) script interface class.

- Added test case `test_update_exlusions` in [particle.py](https://github.com/espressomd/espresso/blob/python/testsuite/python/particle.py) to test updating exclusions.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `0ecfa222a821`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.