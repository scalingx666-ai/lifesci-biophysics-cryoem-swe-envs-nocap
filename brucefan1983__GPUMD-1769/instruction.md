# Issue

**Title:** simplify compute function

## Description

### Summary

Refactor `Force::compute()` internals to reduce duplicated code while preserving the existing public interfaces and computational behavior.

### Changes

* Extract shared force-computation logic into private helper functions.
* Unify potential evaluation and multiple-potential handling.
* Isolate HNEMD and FCP force corrections.
* Keep both existing public `Force::compute(...)` interfaces unchanged.

### Validation

Full GPUMD regression suite passed:

* 195 passed, 0 failed
* Relations: 23 passed, 0 failed, 0 skipped

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `39961ff712df`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.