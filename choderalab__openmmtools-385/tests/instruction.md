# Issue

**Title:** Fix bug when computing reduced potential in multiple states

## Description

This fixes a bug affecting ``reduced_potential_at_states()`` when computing the reduced potential of systems in different ``GlobalParameterState``s when the same alchemical parameter appeared in force objects split in different force groups.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `4616b21a16ea`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.