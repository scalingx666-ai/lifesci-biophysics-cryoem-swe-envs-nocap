# Issue

**Title:** Python versions should be strings in YAML

## Description

Reduce likelihood of mistakes when adding 3.10 to the matrix -- getting ahead of this: https://twitter.com/choldgraf/status/1446666661650599936

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `f2c4c1aa0700`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.