# Issue

**Title:** feat: support for chunked graph server authentication

## Description

* store credentials in file named `chunkedgraph-secret.json`, eg: `{"token":"<auth_token>"}`
* token will be in `auth_header` dictionary in meta class

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `2f8e06efed43`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.