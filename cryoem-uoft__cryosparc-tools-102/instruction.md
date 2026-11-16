# Issue

**Title:** Release v4.6.1

## Description

- Added: Python 3.13 support
- Fix: Disallow non-URL-encodable characters when creating external job inputs and outputs
- Fix: Prevent queuing and killing external jobs (must use `job.start/job.stop()` or `with job.run()`)

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `96aeb7fb78b4`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.