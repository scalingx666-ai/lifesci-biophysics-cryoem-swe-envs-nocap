# Issue

**Title:** CI miscellaneous fixes

## Description

## Description

* Allow codecov to fail without making our CI tests error out. This is desirable because codecov availability can be flaky, so we don't want to depend on it for our CI tests to pass.
* We don't need to pass `openmmtools >=0.23` as extra specs. Test environment file updated accordingly.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `06bbd504577d`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.