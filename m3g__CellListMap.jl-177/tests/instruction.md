# Issue

**Title:** Remove OrthorhombicCell inverse rotation

## Description

I was optimising Molly with AI tools and CellListMap seems pretty well-optimised, so thanks for that.

The following change was suggested and seems reasonable to me, it seemed to give a marginal speedup though the benchmark was noisy on my machine.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `2b27c928fcd5`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.