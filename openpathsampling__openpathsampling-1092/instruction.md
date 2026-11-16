# Issue

**Title:** Actually check for CVDefinedVolume.lambda_max == "inf"

## Description

while looking at the correctness of the docstring in #1068 I noticed a tiny bug in CVDefinedVolume.

fixed bug in 8ea8d38
added test in 11c4119
dropped nose from test_volume.py in 41ffb9b

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `ffe5fa0a5a79`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.