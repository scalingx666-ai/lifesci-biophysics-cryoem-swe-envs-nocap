# Issue

**Title:** Rework harmonic oscillator free energy tests for multistate samplers to run on travis

## Description

## Description
This PR reworks the harmonic oscillator free energy tests for multistate samplers, testing them both with and without unsampled states and ensuring they are fast enough to run on travis ().

I've also converted the `warnings.warn` messages about the experimental API to `logger.warn` so they can be more easily suppressed with the logger level.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `8ca9fb07627f`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.