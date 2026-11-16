# Issue

**Title:** Update CI for OPS 2.0

## Description

The main development branch CI is in `.github/workflows/tests.yml`. The CI for the 2.0 development branch is in `.github/workflows/test-2.0.yml`. This PR ports improvements in `tests.yml` into `test-2.0.yml` (and, of course, targets the `dev-2.0` branch).

Useful improvements:

* Adds Python 3.8 and 3.9 to the test suite. This wasn't possible (no OpenMM available) before.
* Adds notebook tests. I'm not sure why this wasn't done before, but with core changes on the way, such as #1042, I'll feel a lot better having the more thorough test suite.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `dd47ec0bb5a9`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.