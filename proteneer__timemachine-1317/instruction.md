# Issue

**Title:** Speed-up connected_core MCS by 2-3x

## Description

This PR speeds-up the connected_core MCS by improving the data-structures and removes the use of `networkx` to perform BFS search (with a cheap early termination check). The defaults to the test_all_pairs test are also updated to reflect changes that are typical of production.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `b90b37c42d53`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.