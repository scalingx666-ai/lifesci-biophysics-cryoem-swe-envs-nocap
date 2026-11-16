# Issue

**Title:** exec runner: carry live dumps across segment boundaries in append mode

## Description

n_print_steps_equilibration (#272) only works under the library runner. Under the executable runner every mode=fe run dies.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `5add579c8812`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.