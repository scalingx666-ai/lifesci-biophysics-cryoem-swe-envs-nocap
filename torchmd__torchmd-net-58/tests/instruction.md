# Issue

**Title:** Fix accidental bias in equivariant Transformer

## Description

This fixes the bug reported in #57 and adds tests for this case.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `69301dbaccda`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.