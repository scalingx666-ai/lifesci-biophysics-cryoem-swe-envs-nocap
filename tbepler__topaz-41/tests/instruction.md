# Issue

**Title:** Store trained model in models for denoise_image function

## Description

Training denoising models cannot be completed because models is not defined when pretrained models are not used, and models is required for the denoise_image function.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `55ec396ba1ae`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.