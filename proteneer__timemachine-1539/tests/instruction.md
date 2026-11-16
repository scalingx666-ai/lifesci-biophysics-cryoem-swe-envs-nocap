# Issue

**Title:** Move fixed-output tests to separate CI job

## Description

Addresses some pain points with the current organization of fixed-output (i.e. stable hash) tests in CI:
1. hash diffs are not printed until all tests have run (currently ~6 hours for nightly tests)
1. in cases where we expect hash diffs, we'd like the fixed-output tests to start running immediately for faster feedback

A partial workaround could be overriding `PYTEST_CI_ARGS` in the pipeline, but this is a manual step that must be remembered.

This PR moves all fixed-output tests to a separate (non-nightly) CI job.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `5524f188925c`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.