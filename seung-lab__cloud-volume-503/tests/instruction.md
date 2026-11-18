# Issue

**Title:** BREAKING: refactor(MultiLevelPrecomputedMeshManifest): add writing

## Description

Can now read and write the binary format.

The breaking change is the change to the `cloudvolume.datasource.precomputed.mesh.multilod.MultiLevelPrecomputedMeshManifest` interface, but that should be used nearly exclusively internally.

- Fixes bug in spatial_index
- Adds mysql support for spatial_index
- Adds types to CV
- Add support for preflight (OPTIONS) requests to microserver (support sharded)
- Adds forward/backward quantization support for MultiRes meshes

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `ce21879442af`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.