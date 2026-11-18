# Issue

**Title:** Confidence bounds for regression tests

## Description

Regression is now evaluated with a single sample t-test, i.e. the baseline times = conf bounds. This is much nicer imo and giving a confidence region for tests to pass should also make it more robust to potential runtime fluctuations on the github runners.

Also made a few smaller follow up fixes for #475.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `dd477f27192d`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.