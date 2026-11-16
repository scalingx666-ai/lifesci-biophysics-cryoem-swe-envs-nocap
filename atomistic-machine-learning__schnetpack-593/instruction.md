# Issue

**Title:** hydra: add _self_ to defaults list to avoid warning

## Description

Hydra 1.1 changes the order in which values in the defaults list are overridden:

https://hydra.cc/docs/1.2/upgrades/1.0_to_1.1/default_composition_order/

None of the configs I found in schnetpack is affected by this but in order to make it clear to hydra that we're aware of this issue, we can add _self_ at the beginning of the defaults list. This avoids a warning on every run.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `ca2849c090bd`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.