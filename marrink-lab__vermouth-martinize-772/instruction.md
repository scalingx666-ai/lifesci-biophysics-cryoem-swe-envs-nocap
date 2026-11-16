# Issue

**Title:** Migrate to pyproject.toml

## Description

Time doesn't pause. Migrate the build and install config to the more modern pyproject.toml. Also exclude tests (and some other development files) from the distributed code.
Part of this is also moving bin/martinize to inside the vermouth package.

- [x] Fix fetch depth in CI actions
- [x] Fix no-local-version-strict -> fail-on-uncommitted-changes
- [x] Fix RTD config

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `bfa94b2f1a3d`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.