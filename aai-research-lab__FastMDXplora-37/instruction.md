# Issue

**Title:** Terminal Banner

## Description

## Summary

Improved the FastMDXplora terminal banner to provide a clearer overview of each run before execution. The banner now displays key setup and simulation parameters, planned simulation stages (NVT, NPT, production, and total steps), reporting outputs, and uses the project's actual default values instead of hardcoded settings where appropriate.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `aad1c08d8e02`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.