# Issue

**Title:** Switch to conda-rc-check@v2

## Description

This has been failing for a while (the old web-scraping approach was fragile), so I just cut a new release of the action with a better implementation. This PR updates our workflows to use the newer versions.

Updated checkout on this action, too.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `f63ca01a1ca8`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.