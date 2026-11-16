# Issue

**Title:** Match FastMDAnalysis hydrogen-bond counting in compatibility mode

## Description

- Add configurable candidate frequency and count multiplier to H-bond analysis
- Match FastMDAnalysis transient-bond and two-sided counting behavior
- Keep existing FastMDXplora defaults unchanged
- Update the compatibility profile and regression tests

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `026acbe8c4f0`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.