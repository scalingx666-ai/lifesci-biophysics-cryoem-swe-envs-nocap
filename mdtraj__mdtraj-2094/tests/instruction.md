# Issue

**Title:** rewrite trajectory.py topology parsing with match/case

## Description

Since we (and the world) dropped python 3.9 support, I rewrote a huge block in trajectory.py with match/case instead of if/elif because it was really wordy. 

Lengthwise it's the same (mostly because I divided a block into two due to a line being too long), but it's cleaner, easier to read with less characters and does less checks, not that it should affect efficiency too much.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `8edbc7330311`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.