# Issue

**Title:** -el fix

## Description

Currently, if elastic network rubber bands are applied and are 1) below the cutoff for -el and 2) not otherwise eliminated by the -ermd threshold for the force field, they are kept in. Ie. the -el cutoff is not actually currently applied. This PR applies the cutoff.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `1593bf140277`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.