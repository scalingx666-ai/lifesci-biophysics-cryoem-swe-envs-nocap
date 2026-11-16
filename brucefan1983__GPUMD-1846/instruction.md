# Issue

**Title:** fix one bug and optimize performance

## Description

### Summary

PRD now detects escapes only at block ends (discrete-time ParRep with a time step equal to the `event` interval) and no longer replays the block step by step. This gives all replicas the same stopping rule and removes the per-step quench cost of the replay. `event 1` behaves as before.

### Changes

- `src/main_replica/replica_prd.cu`
- `tests_regression/replica/test_replica.py`
- `doc/gpumd-replica/input_parameters/multi_replica_prd.rst`
- `doc/gpumd-replica/input_parameters/run.rst`
- `doc/gpumd-replica/output_files/prd_events.rst`

### Validation

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `2c93346fce44`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.