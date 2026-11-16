# Issue

**Title:** Add method to get samples from context without storing to disk

## Description

# Changes

* Minor changes to the StoredArrays object around memory usage
* 5% speed up of Vacuum HREX runs, 1% for solvent

# Benchmarks
Cuda arch 8.6
Nvidia A10

## Vacuum
For 48 windows of vacuum the ns/day went from 5919 -> 6318, a ~7% speed up.

### Master
![vacuum_100_benchmarks_master](https://github.com/proteneer/timemachine/assets/5840832/7b85e747-fc92-4abc-922f-16731fb683d3)

### Changes
![vacuum_100_benchmarks](https://github.com/proteneer/timemachine/assets/5840832/fdcbe32f-8e10-46a7-ace0-15fa3557ace9)

## Solvent
For 48 windows of solvent the ns/day went from 900 -> 911, a ~1% speed up.

### Master
![solvent_100_benchmarks](https://github.com/proteneer/timemachine/assets/5840832/345d4e6a-310d-42d0-8e40-0a8a545dba63)


### Changes
![solvent_100_benchmarks_allmem](https://github.com/proteneer/timemachine/assets/5840832/9622c90b-c418-4063-adce-f1bca2d2fd63)

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `624330563c25`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.