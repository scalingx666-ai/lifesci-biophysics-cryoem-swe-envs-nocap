# Issue

**Title:** Fix atom ordering in QTBIntegrator

## Description

.  This fixes an error that could occur in fairly obscure circumstances.  It happened when

- You're using QTBIntegrator.
- You are *not* using particle types, so it determines different spectra for particles that are otherwise identical.
- You're using one of the GPU platforms.
- The system involves periodic boundary conditions.

In that case, atom reordering could cause the spectra for different atoms to get mixed up.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `89b6a36b7489`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.