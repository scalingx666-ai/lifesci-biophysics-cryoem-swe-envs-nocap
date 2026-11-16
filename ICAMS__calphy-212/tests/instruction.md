# Issue

**Title:** Allow full swapping

## Description

This pull request introduces a new feature to allow swapping between all atom types (including fictitious ones) during Monte Carlo simulations, and refactors how swap moves are handled for both forward and reverse passes. The changes improve flexibility for advanced users and make the swap logic more robust and extensible.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `18ee1f70bf44`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.