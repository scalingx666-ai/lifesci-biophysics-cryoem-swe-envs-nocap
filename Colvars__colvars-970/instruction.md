# Issue

**Title:** Refactor NAMD proxy class (preparation for CUDAGM integration)

## Description

This PR refactors `colvarproxy_namd` so that its functionality can be made independent from the `GlobalMaster` class (GM). The following improvements are:
- breaking up some long functions that accumulated over time
- protecting calls that use the `globalmaster` pointer, so that that pointer may be null if a `CudaGlobalMaster` (CUDAGM) object is used instead
- making certain functions' and members' names GM-specific
- distinguish the case where the volmap code is available for direct use by Colvars, vs. GM being available; in the former case, we should still be able to use volmaps when running with CUDAGM
- condensing the code that handles refcounts for atoms that are requested multiple times.

These changes should come with no major feature changes, but merging this PR would reduce conflicts with other PRs later on.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `cf3b224ee315`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.