# Issue

**Title:** Ensure Changelog is updated with every PR.

## Description

I also like the suggestion of @ntolley to make updating the changelog part of PRs. To avoid having to remind contributors on every PR this could be enforced via github actions. I wrote a little check to do this.

(Quick test: changelog not updated -> tests should fail)

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `ea54af414ea1`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.