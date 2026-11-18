# Issue

**Title:** feat: add compresso encoding

## Description

Won't be supported by neuroglancer, but it might be useful for storing on disk when performing computation on segmentation. Compresso is fast and often half the size of compressed_segmentation.

It could also be useful as a wire format for reducing egress costs.

Still have to get binaries building on Windows before releasing.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `83ae55da9ed6`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.