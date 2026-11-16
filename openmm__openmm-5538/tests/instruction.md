# Issue

**Title:** Convert DrudeLangevinIntegrator to LF-Middle discretization

## Description

When we converted other integrators to the LF-Middle discretization, DrudeLangevinIntegrator was missed.  This should make it more accurate and allow larger time steps.  In the process, it also fixes items 1 and 2 from #5480.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `308e446242e3`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.