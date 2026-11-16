# Issue

**Title:** Document GHMCMove performance considerations

## Description

## Description

This adds a short documentation note that `GHMCMove` uses a Metropolized propagator and can be substantially more expensive per integration step than non-Metropolized dynamics moves such as `LangevinDynamicsMove`.

The note also points users to the existing GHMC acceptance statistics (`n_proposed`, `n_accepted`, and `fraction_accepted`) and recommends benchmarking throughput for performance-sensitive workflows.

## Motivation

`GHMCMove` has a similar user-facing constructor shape to `LangevinDynamicsMove`, but it is algorithmically different and may carry meaningful overhead, especially for small systems, short propagation segments, or many-state/multistate workflows.

## Testing

- `python -m py_compile openmmtools/mcmc.py`

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `ba4031da4998`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.