# Issue

**Title:** feat: add cave-secret.json and allow for fqdn qualification

## Description

e.g. flywire.dynamicannotationframework.com-cave-secret.json
(fake example)

Also adds the ability to configure the `.cloudvolume` directory via an environment variable `CLOUD_VOLUME_DIR`

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `7722e3be5c8c`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.