# Issue

**Title:** Remove optional boost_graph dependency

## Description

The Boost branch of DFSCLUSTERING was off by default, never enabled in the CI, and no longer compiled against the current code (retrieveEdgeList/pairs were refactored away). Always use the built-in depth first search. Remove the --enable-boost_graph configure option (configure.ac and the generated configure, edited by hand), update the developer docs example to boost_serialization, and add a CHANGES note.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `41552674d29b`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.