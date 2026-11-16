# Issue

**Title:** feat: enforce slot loading in load_output()

## Description

If a user provides a field name instead of a slot/prefix, convert to the associated slot with a warning. This resolves a common mistake users make of providing e.g.,` "ctf/df1_A"` instead of `"ctf"`.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `04d0b7e663c6`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.