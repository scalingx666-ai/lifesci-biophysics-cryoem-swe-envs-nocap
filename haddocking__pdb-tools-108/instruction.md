# Issue

**Title:** Add GitHub actions for testing

## Description

* Tests Ubuntu, MacOS, Windows
* Tests python 2.7, 3.6 to 3.9
* Runs UnitTests on all platforms/versions (sends report to CodeCov)
* tests package build and `bump2version` on all platforms/versions
* Updates README.rst badges

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `0312bc4f48ac`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.