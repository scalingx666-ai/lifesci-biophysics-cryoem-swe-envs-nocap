# Issue

**Title:** Adaptive nSpacing in Surface DC-PSE

## Description

Adaptive surface DC-PSE. If the options::ADAPTIVE is passed, the nSpacings member is filled with rCuts/nCount (still not fully sure this is the best way but it's a start), then the particles along the normal are created and the Verlet list is updated. Everything else is the same.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `f4cfc705f98b`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.