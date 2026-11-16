# Issue

**Title:** Create environment_blackwell.yml for RTX 5000 PyTorch support

## Description

New `environment.yml` file for CUDA 12.8 (Blackwell) GPU support for BLUSH, Model-Angelo and DynaMight.

Compatible with any nVidia GPU supporting the `nvidia-open` driver, but does not work with pre-Turing GPUs, so do not replace current `environment.yml`.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `85db7351593e`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.