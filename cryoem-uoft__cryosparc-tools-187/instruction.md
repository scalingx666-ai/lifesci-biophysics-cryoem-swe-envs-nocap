# Issue

**Title:** docs: audit controller docstrings

## Description

For improved detail and clarity.
- Added additional `job.queue` arguments
- Improved `job.set_params` with optional keyword arguments
- Improved `download_file` types and behaviour 
- Rename `job.connect` `source_job_uid` argument to `source_job` (old name supported with a deprecation warning)
- Added `job.export` function

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `99b03027008c`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.