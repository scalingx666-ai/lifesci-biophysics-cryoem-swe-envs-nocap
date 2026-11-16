# Issue

**Title:** Register rlnTomoReconstructedTomogramDenoised metadata label

## Description

It is written by the cryoCARE denoise wrapper but was not registered in metadata_label.h (the unused rlnTomoDenoisedTomogram was registered instead), so reading a denoised tomograms.star prints an unknown-label warning.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `fe136118df1e`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.