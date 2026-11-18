# Issue

**Title:** refactor(compressed_segmentation): use updated 2.0.0 package

## Description

This update will streamline the compression phase logic, make the decompression order work as expected and also prevent compressing volumes that are not supported by the specification.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `32e8cfffa4bc`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.