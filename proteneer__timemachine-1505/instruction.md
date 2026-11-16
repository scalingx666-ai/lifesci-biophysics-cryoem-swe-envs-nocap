# Issue

**Title:** Remove deprecated tmpdir usage

## Description

* tmpdir is deprecated: https://docs.pytest.org/en/stable/how-to/tmp_path.html#temporary-directory-location-and-retention
* Speed up a test that doesn't need to run 1000 steps

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `76c1fc1bcde0`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.