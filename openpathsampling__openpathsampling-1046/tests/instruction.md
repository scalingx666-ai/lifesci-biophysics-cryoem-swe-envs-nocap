# Issue

**Title:** Prevent errors in ShootingPointAnalysis with invalid (e.g., max length) trajectories

## Description

A student tried to use ShootingPointAnalysis on a comittor simulation. However, due to the nature of his (biological) system, some of the steps were rejected due to reaching maximum length.

He thus ran into the (leftover) AssertionError.

This PR cleans up that error handling and adds an option to skip rejected steps:
e46fe67 adds the functionality and tests
2a529c4 solves a pep8 complaint from my coding environment
37e5428 drops nose from the test file and cleans up some unnecessary compatibility code (this test file should be inspected again when dropping python 2.7)

For 2.0 I would propose to break API and change the default of `skip_rejected` to `True`. (I think that rejected steps should not be included in analysis on default).

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `dd9b5e1159e6`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.