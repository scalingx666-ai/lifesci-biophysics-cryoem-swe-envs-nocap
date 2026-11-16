# Issue

**Title:** Minor changes to maintenance script to parse GitHub metadata

## Description

Some fixes and updates to the script to allow listing the individual commits from PRs after their branches are rebased onto master.

Note: this requires that for each PR the original branch is available in the repo, because the GH CLI returns the *original* commit hashes, not the rebased ones.  It would be useful if we kept around the branches for bugfix PRs until the step of cherry-picking into release branches is automated.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `5ab2dfebfb44`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.