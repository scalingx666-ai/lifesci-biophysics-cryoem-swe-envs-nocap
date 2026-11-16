# Issue

**Title:** fix(triples): fix triples computation

## Description

I believe there was a problem in the computation of atom triplets. This problem only occurred when the sequence in `idx_i` was non-consecutive (i.e. something like `0, 1, 1, 3, 3, 3` instead of `0, 1, 1, 2, 3, 3`.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `ceef75ec281a`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.