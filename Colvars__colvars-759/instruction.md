# Issue

**Title:** min_image fix; Saves long runs from crashes;

## Description

This addresses [this](https://matsci.org/t/fix-rigid-atoms-have-moved-too-far-apart/53926/14) recent issue solution

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `d8b5a7f7d4cb`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.