# Issue

**Title:** Switch to pytest runner

## Description

As a first pass in improving our testing, we are going to switch to using [`pytest`](https://docs.pytest.org/en/stable/) to run tests. Because of the [compatibility between `pytest` and `nose`](https://docs.pytest.org/en/stable/nose.html) we shouldn't need to do anything to the old tests, but we should start writing new tests using `pytest` features.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `317a3bbfdc79`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.