# Issue

**Title:** Stop attempting "legacy editable" installs

## Description

Legacy-editable mode was removed in pypa/setuptools#4953, this env
variable no longer does anything. We accommodate the new behavior in
cctbx/dxtbx#807 and dials/dials#2906.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `5f2749112e75`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.