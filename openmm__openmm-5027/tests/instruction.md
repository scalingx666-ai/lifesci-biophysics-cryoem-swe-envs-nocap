# Issue

**Title:** Reducing sorting overhead for small systems with PME

## Description

Implements #4982.

When simulating small systems (up to 15,000 atoms) with PME, it only sorts the particles every fourth step.  The result is a significant though not huge speedup for very small systems (around 1000) atoms.  It shrinks for larger systems, and mostly disappears by about 15,000 atoms, which is why I set the cutoff there.

cc @dmclark17

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `1eabb5a61341`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.