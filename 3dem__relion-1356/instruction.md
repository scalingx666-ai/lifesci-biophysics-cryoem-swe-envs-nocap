# Issue

**Title:** pyproject.toml: add scipy and dill, used directly but never declared

## Description

get_particle_poses/{spheres,filaments}.py import scipy directly, and scripts/filament_selection.in does `import dill as pickle` - both only ever installed by accident, transitively, via other packages that happen to depend on them.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `f6d3ad20cde0`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.