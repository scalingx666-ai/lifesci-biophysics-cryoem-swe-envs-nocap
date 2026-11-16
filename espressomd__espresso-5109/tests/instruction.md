# Issue

**Title:** Improve resilience to precision loss

## Description

Description of changes:
- properly fold coordinates in the histogram and RDF classes to avoid off-by-one errors
- adjust test tolerances to better reflect recent changes to waLBerla and heFFTe algorithms

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `4989e9e0eb5d`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.