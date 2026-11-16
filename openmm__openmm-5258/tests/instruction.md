# Issue

**Title:** Retry error when checking for broken links

## Description

The docs CI build frequently fails while trying to check for broken links.  I think this is usually due to network errors.  This attempts to make it more reliable by rechecking them and only reporting an error if they fail repeately.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `9aae4bb35c53`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.