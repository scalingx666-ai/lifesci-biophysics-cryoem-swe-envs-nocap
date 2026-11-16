# Issue

**Title:** Change timeseries to only consider a subset of nodes for equilibration

## Description

The analyze modules now will only consider up to 1000 points by default in the timeseries fed into them for equilibration. This is a tunable parameter.

Added warning for future removal of the `get_equilibration_data` in `multistate.utils`

The mixing stats do not print out by default anymore when you invoke `analyze_phase`

All of these changes should make analysis faster.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `50dfda55b2db`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.