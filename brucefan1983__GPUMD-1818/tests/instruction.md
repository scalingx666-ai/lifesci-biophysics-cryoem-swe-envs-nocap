# Issue

**Title:** Add optional trajectory file splitting to dump_xyz

## Description

## Motivation

Long MD simulations can generate very large trajectory files, making them inconvenient to download from HPC systems, transfer, and store.

To make trajectory management more convenient, we propose allowing users to specify the maximum number of frames stored in each output file, without interrupting the simulation.

## Implementation

This PR introduces an optional `split` parameter to `dump_xyz`. For example:

```text
dump_xyz 100 trajectory.xyz split 10000
```

This saves one frame every 100 MD steps and automatically creates a new XYZ file after every 10,000 frames:

```text
trajectory_0001.xyz
trajectory_0002.xyz
trajectory_0003.xyz
...
```

The implementation preserves the original `dump_xyz` behavior when `split` is not specified. It also avoids overwriting existing trajectory files and includes documentation and a regression test.

## Testing

The functionality was tested on an NVIDIA H100 GPU using an adapted GPUMD v5.7 build. File splitting, legacy output modes, and consecutive runs were verified. GPU integration testing against the latest `master` remains pending.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `b9d6f89e6658`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.