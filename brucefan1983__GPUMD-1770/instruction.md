# Issue

**Title:** Refactor Force responsibilities

## Description

### Summary

Refactor `Force` internals to clarify responsibilities and reduce method-specific logic.

### Changes

* Simplify potential creation in `Force`.
* Move HNEMD and HNEMA driving-force logic to the corresponding actions.
* Move HNEMDEC driving-force logic to the HNEMDEC action.
* Move FCP total-force correction into the FCP potential and reuse its GPU workspace.
* Clean up obsolete `Force` state.

### Validation

The full GPUMD regression suite was used throughout the refactoring.

The HNEMD and HNEMDEC tests initially showed small numerical differences because, after this refactoring, the driving force is no longer included in the initial force evaluation. Instead, it is applied by the corresponding action during subsequent MD steps. This slight change in timing is intentional and physically acceptable.

After accepting this behavior change and updating the baseline executable, the full regression suite passes:

* 195 passed, 0 failed
* Relations: 23 passed, 0 failed, 0 skipped

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `1f9b4a0de2b9`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.