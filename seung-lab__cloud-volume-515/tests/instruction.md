# Issue

**Title:** feat: add cv.unique

## Description

`cv.unique` allows you to query arbitrarily large bounding boxes and extracts the unique labels in an online fashion to avoid excessive memory consumption. 

I also exploit the cseg and compresso formats so that the extraction can occur after only the first stage of decompression for additional speed.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `9cb098efd7d3`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.