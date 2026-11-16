# Issue

**Title:** fix(spm): correctly parse pydantic model results

## Description

Use `model_validate(data)` instead of `__init__(**data)` when parsing JSON from the API to ensure that special model validators which return a different type have their results interpreted correctly by pydantic. This affects event models.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `7c5566b68bd9`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.