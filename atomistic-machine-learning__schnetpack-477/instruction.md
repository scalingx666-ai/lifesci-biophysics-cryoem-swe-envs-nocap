# Issue

**Title:** Updated docs and fixed problems

## Description

Updated docs to include the MD API and fixed problematic references.
Moved class properties in simulation hooks and integrators beneath docstrings for summaries to render properly.

Updated the `howto_batchwise_relaxations` tutorial to use `get_relaxation_results` instead of the deleted `get_relaxed_structures`.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `37fab8d5b93b`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.