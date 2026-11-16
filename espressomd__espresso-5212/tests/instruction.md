# Issue

**Title:** Make VS relative thread-safe

## Description

Make the back-applications of forces/torques serial to avoid the race condition when adding forces/torques from several vs to the same reference particle. The test is extended from 2 to 100 virtual sites, such that it would be likely to catch a race condition (and does without the fix).

An optional `bool parallel` parameter to `CellStructure::for_each_local_particle()` was added, with true as default. This way, parallelism can be disabled for individual calls.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `d7811f01b753`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.