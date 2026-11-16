# Issue

**Title:** Progress indicator for shooting point analysis

## Description

This adds progress indicators (i.e., tqdm support; introduced in #882) for the shooting point analysis. With tqdm installed, you automatically get progress bars when doing the analysis.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `647ceb05bbcc`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.