# Issue

**Title:** Use new testfiles and increase coverage to 100% for amber parser

## Description

I removed the dependency to load_invalidfiles and replaced it with the new load_testfiles, now I removed all  # pragma and the coverage for the parser should be 100%

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `4d5cd21c7216`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.