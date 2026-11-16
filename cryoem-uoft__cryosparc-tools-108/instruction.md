# Issue

**Title:** feat(spm): initial REST API client for CryoSPARC

## Description

Uses OpenAPI schema to generate methods API instead of JSONRPC. A future pull request will include a type stub for the generated methods.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `086bda8591c4`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.