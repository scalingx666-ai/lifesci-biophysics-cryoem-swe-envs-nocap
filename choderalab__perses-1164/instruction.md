# Issue

**Title:** Avoid testing non-examples in CI

## Description

## Description

From #1145 our examples directory was populated with many scripts that are not necessarily ready-to-run examples.

These changes avoid trying to run these as part of our CI, solving errors on running tests.

## How has this been tested?

Tested locally running `test_examples.py` tests.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `12d8b212f5f5`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.