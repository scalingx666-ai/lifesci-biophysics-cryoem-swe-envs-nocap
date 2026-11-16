# Issue

**Title:** Extend default compatibility of integrators

## Description

This adds the optimization discussed in #352. The state of the integrator was always consistent even before this change, but now the compatibility of the integrator has been extended by default to all global variables, so new `Context` objects are created less often.

This also adds a blacklist `ContextCache.INCOMPATIBLE_INTEGRATOR_ATTRIBUTES` that can be used to exclude some integrator parameters from the copy when requesting the cached context.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `fde2b694469f`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.