# Issue

**Title:** [SKIP] re-work `bump-version-on-push.yml`

## Description

~This PR removes the `bump-version-on-push.yml` - while it's a nice implementation it is also outdated and we should use a similar pipeline to the other codes from the group.~

This PR re-works the `bump-version-on-push.yml`, replacing the deprecated `bump2version` and changing the way the versions are pushed to pypi

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `ffc490e04974`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.