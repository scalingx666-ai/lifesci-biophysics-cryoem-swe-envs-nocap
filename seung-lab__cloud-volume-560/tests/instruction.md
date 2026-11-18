# Issue

**Title:** feat(mesh): add put and delete to mesh objects

## Description

put only implemented for unsharded legacy meshes
delete only implemented for unsharded multires and legacy meshes

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `89599245fde6`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.