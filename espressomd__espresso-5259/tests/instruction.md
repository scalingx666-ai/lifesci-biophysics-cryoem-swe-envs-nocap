# Issue

**Title:** LBWalberlaImpl readability

## Description

The LBWalberlaImpl tempalte has gotten too long to be understandable by new people. This is WIP to improve on that by moving out some details to separate files and re-order the main class template.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `95f275aa2060`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.