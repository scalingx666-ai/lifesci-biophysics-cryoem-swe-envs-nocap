# Issue

**Title:** add AUTHORS and CHANGES

## Description

- close #83
- close #84
- files were manually created based on the history (git log --format="format:%an %ad" --date="format:%Y" 0.1.0..HEAD | sort | uniq)
  and the merged PRs and closed issues

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `d3ea8580e2ed`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.