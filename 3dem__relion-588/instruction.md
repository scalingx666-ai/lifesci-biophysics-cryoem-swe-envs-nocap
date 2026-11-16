# Issue

**Title:** PORT: Expectation setup  to  GPU

## Description

This part doesn't touch any FFT stuff. Moving FFT part to GPU will also reduce some data transfers.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `5997001f75ab`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.