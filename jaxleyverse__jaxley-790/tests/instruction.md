# Issue

**Title:** fix changelog chore

## Description

Changelog check now diffs against base of the merge, rather than main. Prev., main could be ahead and hence had a diff with pr branch leading to a passed changelog check, without the PR actually introducing changes to the changelog.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `c222cae7e46f`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.