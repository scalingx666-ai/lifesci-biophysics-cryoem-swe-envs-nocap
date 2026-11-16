# Issue

**Title:** Add constraint propagation to ISMAGS

## Description

This PR adds constraint propagation to the ISMAGS algorithm, as per the original paper. This is a good thing(tm) thing because it means constraints can help shape the search space.
Also, this PR looks worse than it is. The first commit only shuffles methods around, but doesn't change any code. The second commit actually makes the change.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `7098b95ccf01`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.