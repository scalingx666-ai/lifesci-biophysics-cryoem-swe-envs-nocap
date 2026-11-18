# Issue

**Title:** Enable param sharing with differently sized groups

## Description

This PR attempts to enable param sharing for controll groups with different numbers of members. This can be important for sharing params between branches with different numbers of segments or edges with different numbers of pre synaptic cells ().

This also .

todos:
- [x] add tests
- [x] rm asserts that prev. prevented the user from doing this.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `a6c19ab2e74d`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.