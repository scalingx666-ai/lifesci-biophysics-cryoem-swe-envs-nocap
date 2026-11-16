# Issue

**Title:** Avoid incorrectly adding plasma correction

## Description

This fixes an edge case in computing the neutralizing plasma self energy.  If a NonbondedForce included offsets, then the GPU platforms would always include the plasma self energy, even when you requested a different force group.

.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `58378cceac47`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.