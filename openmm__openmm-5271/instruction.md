# Issue

**Title:** Avoid host wait for PME post-computation synchronization

## Description

This replaces the host-side wait in `SyncQueuePostComputation` with a queue-side wait.

When PME runs on a separate queue, the post-computation step only needs to ensure that later work on the default queue does not run until the PME queue has completed. Enqueuing a wait on the current queue preserves that dependency, while avoiding a CPU-side stall. The `addEnergy` kernel is still launched after the wait, so the PME energy buffer is not consumed until the reciprocal-space work has finished.

Before the common NonbondedForce implementation in #4922, the CUDA, HIP, and OpenCL implementations used queue/stream-side synchronization for this step (`cuStreamWaitEvent`, `hipStreamWaitEvent`, and an OpenCL barrier wait list). This change restores the same synchronization pattern through the common `ComputeEvent::queueWait()` API.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `46376ea3e4d0`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.