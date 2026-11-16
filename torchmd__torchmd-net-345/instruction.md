# Issue

**Title:** Add backup for metrics.csv

## Description

We currently use `CSVLogger` from `torch.lightning` to write metrics to a CSV file in the log directory. However, when using `load_model` (i.e., loading from a checkpoint), or more in general to append data to an existing CSV, we overwrite the current metrics.csv, losing the original data.
This PR introduces a simple function that checks if the metrics file already exists and renames it using date and time information, preserving previous data. 
Ideally, we would prefer to append to the existing file, but AFAIK this is not currently supported by CSVLogger.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `f10cc8978d91`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.