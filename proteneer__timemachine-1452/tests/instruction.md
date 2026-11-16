# Issue

**Title:** Convert uM to kJ using same temperature as simulations

## Description

* Removes an inconsistency between the simulation temperatures and the conversion of experimental uM/kD values to kJ.
* Should investigate changing DEFAULT_TEMP to `298.15` to match experiments, but requires more validation.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `e737b868aae4`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.