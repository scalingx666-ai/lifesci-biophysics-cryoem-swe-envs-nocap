# Issue

**Title:** Probe2 json

## Description

Fixes some bugs in probe2 so that it can load more files, and prints more robust errors when it fails.
Adds the ability to export a JSON format to probe2 with the same data as the raw format but also able to handle CIF files.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `a95b1909ece1`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.