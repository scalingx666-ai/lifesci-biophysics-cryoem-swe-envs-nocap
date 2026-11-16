# Issue

**Title:** Add option to run External through a CUDA graph

## Description

This PR allows to capture a TorchMD-Net model to run it in TorchMD.

A new argument is added to external, use_cuda_graphs that defaults to False

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `f837503b9dae`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.