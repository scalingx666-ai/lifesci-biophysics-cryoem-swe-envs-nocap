# Issue

**Title:** npy file for embedding

## Description

In this pull request, I propose the utilization of an npy file for loading the embedding rather than a list. This enhancement enables running the code by providing either the path to the embedding.npy file or a list of integer values (retro-compatible). This modification proves highly beneficial, especially when working with bigger molecules or protein sequences.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `bd447a390334`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.