# Issue

**Title:** Update YANK with new OpenMMTools

## Description

A couple of fixes necessary after the new OpenMMTools 0.17.0 release.
- Fix #1133: Remove reference to AlchemicalState(update_alchemical_charges), which was dropped in OpenMMtools 0.17 with the new exact PME scheme.
- `RestraintState` inherits from `openmmtools.states.GlobalParameterState`. This removes the deprecated warnings and add support for the kwarg `parameters_name_suffix`, which is necessary for the multi-restraint feature (this fix was already done in the `multirestraint` branch, but it'll take a little for that to be merged into master).

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `63290159a196`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.