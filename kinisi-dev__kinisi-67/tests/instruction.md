# Issue

**Title:** Add MSD test to aid future development

## Description

Added a known MSD calculation to tests. This should allow us to maintain consistency of basic MSD calculations through future development.

This branch also fixes einsum for NPT unwrapping and adds a new mdanalysis example (based on the new test case) to the docs.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `b096eab8a5c1`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.