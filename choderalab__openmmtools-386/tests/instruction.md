# Issue

**Title:** Set exact PME as default

## Description

- Set exact PME as the default value for `alchemical_pme_treatment` in the `AbsoluteAlchemicalFactory` constructor.
- I've added an optional `ignore_velocities` argument in the `__setstate__` and `__getstate__` methods of `SamplerState`. This will allow us to generalize in YANK the way MPI process send back the info about the `SamplerState` to the root process (including collective variables) without wasting bandwidth.
- I'm also checking if using the CPU platform to create `Context`s fixes the random segfaults in the Travis tests.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `10af5a65b989`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.