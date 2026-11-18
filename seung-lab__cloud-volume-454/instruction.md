# Issue

**Title:** feat: add support for base64 encoded token

## Description

currently only hexadecimal strings are supported for graphene auth tokens, this adds support for base64 encoded strings
regex used for this is from [here](https://base64.guru/learn/base64-characters ) (single line base64 string)

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `2bebb8be4875`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.