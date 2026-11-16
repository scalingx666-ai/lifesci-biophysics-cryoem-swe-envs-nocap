# Issue

**Title:** ASA calculation should be done per model

## Description

With this the default `AsaCalculator` constructor will use model 1 only. Also introduced a new constructor to choose the model.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `be55381c8b96`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.