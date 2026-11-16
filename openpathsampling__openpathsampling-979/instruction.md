# Issue

**Title:** Move n_dofs to engine; add engine.has_constraints()

## Description

This moves the calculation of the number of degrees of freedom to the engine. This is expected to be a constant for an engine -- at least, the engines we have so far don't allow number of particles to change -- so it makes sense to cache it (for OpenMM), instead of calculating it over and over with the `snapshot.n_degrees_of_freedom` feature.

This also adds an `engine.has_constraints()` method. This gives a cleaner way to abort if an engine with constraints tries to use one of the `DirectionModifier` snapshot modifiers.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `82d5948dfe6f`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.