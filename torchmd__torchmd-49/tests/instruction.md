# Issue

**Title:** fix some errors

## Description

- [x] move `batch_comp`  from global variable to function args
- [x] include `repulsion `and `repulsioncg `in build_parameters function. I removed 'LJ' since we have term.lower() for all the terms.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `b87c585c3aca`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.