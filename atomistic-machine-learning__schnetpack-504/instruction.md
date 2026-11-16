# Issue

**Title:** Removed deprecated arguments …

## Description

…'amp_backend', 'amp_level', and 'resume_from_checkpoint' from trainer config. Added 'ckpt_path' to run config as a replacement for 'trainer.resume_from_checkpoint' and adapted the cli script accordingly.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `6b98a857d470`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.