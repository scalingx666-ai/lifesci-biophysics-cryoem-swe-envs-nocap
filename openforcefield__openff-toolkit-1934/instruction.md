# Issue

**Title:** Remove the FAQ and redirect to ecosystem-wide FAQ

## Description

See openforcefield/openff-docs#63, #1932. .

This doesn't add a redirect from the current FAQ URL to the new one - I'll add that by hand in RTD when I merge this.

- [x] Update [changelog](https://github.com/openforcefield/openff-toolkit/blob/main/docs/releasehistory.md)

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `fd7b8b0ec545`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.