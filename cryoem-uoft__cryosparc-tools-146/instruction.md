# Issue

**Title:** feat: update functionality for job.log with name argument

## Description

Update an existing log by providing a name for it. Can use the ID of a previously-logged event as a name. Named events are reset when adding a checkpoint.

For example, show a live progress bar in the job log.
```py
for pct in range(1, 10):
    # example log: "Progress: [#####-----] 50%"
    job.log(f"Progress: [{'#' * pct}{'-' * (10 - pct)}] {pct * 10}%", name="progress")
    sleep(1)
```

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `ddbf2bdf1527`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.