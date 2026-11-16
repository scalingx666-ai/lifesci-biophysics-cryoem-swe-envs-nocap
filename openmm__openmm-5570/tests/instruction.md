# Issue

**Title:** Fix empty nonbonded work ranges on GPU platforms

## Description

Fix stale neighbor-list interaction counts when a GPU's assigned nonbonded block range becomes empty, and consistently use `numBlocks` to determine whether neighbor-list preparation and count synchronization are required.

Related to #5539. This implements the changes discussed with @peastman.

## Changes

Apply the corresponding corrections to CUDA, HIP, OpenCL, and Metal.

### Clear counts for an empty block range

At the end of `setAtomBlockRange()`, reset `interactionCount` when `numBlocks == 0`, provided the array has already been initialized.

For CUDA, HIP, and Metal:

```cpp
if (numBlocks == 0 && interactionCount.isInitialized()) {
    vector<unsigned int> count(2, 0);
    interactionCount.upload(count);
}
```

OpenCL uses a one-element array:

```cpp
if (numBlocks == 0 && interactionCount.isInitialized()) {
    vector<cl_uint> count(1, 0);
    interactionCount.upload(count);
}
```

The initialization check is necessary because `setAtomBlockRange()` is also called before the count array is allocated.

### Use block counts for neighbor-list guards

In `prepareInteractions()`:

```diff
-    if (numTiles == 0)
+    if (numBlocks == 0)
         return;
```

In `computeInteractions()`:

```diff
-    if (useNeighborList && numTiles > 0) {
+    if (useNeighborList && numBlocks > 0) {
```

The neighbor-list kernel receives the block range, while `numTiles` is derived from a different integer partition. A sufficiently narrow range can therefore have `numBlocks > 0` but `numTiles == 0`.

For Metal, update the event-enqueue condition in `prepareInteractions()` as well, keeping event registration consistent with the wait condition:

```diff
-    if (useNeighborList && numTiles > 0)
+    if (useNeighborList && numBlocks > 0)
         event->enqueue();
```

Force-kernel execution is preserved, including the exclusion-related work that may remain on a context with an empty dynamically assigned block range.

## Validation scope

The CUDA changes were previously tested in a locally rebuilt OpenMM 8.5.2 plugin on four NVIDIA GH200 GPUs. The reproducer and matching-coordinate force comparisons are documented in #5539.

The current PR commit based on `master` has not yet been built or runtime-tested locally. Additional CUDA checks in mixed and double precision are pending; single precision will not be tested.

HIP, OpenCL, and Metal have not been built or runtime-tested. No new automated regression test is included in this PR.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `146a320f97bd`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.