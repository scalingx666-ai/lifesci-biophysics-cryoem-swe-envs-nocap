# Issue

**Title:** Clear deferred-free state when the custom GPU allocator frees a block

## Description

.

`_free()` marks a block free but keeps its `freeWhenReady` flag and completed `readyEvent`, which `AccPtr::freeDevice()` sets on most frees. Since #1369, `alloc()` can hand that block back out on an exact fit, so the next `_freeReadyAllocs()` frees it again under its new owner, and the owner's own free then hits a block that was already freed or merged away.

This destroys the event and clears `freeWhenReady` in `_free()`, so a free block carries no pending deferred free. The HIP allocator has the same code and gets the same change; only CUDA was tested.

**Testing:** the command from #1374 on an NVIDIA L40S, with RELION built from 108ef3cb25b3ad341a025eedeb3ad538884c1852 (allocator code identical to the current `ver5.1`), CUDA 12.8 and gcc 12.4. Without this patch it segfaults in `CudaCustomAllocator::_free` in the first expectation iteration; with this patch the refinement runs through.

🤖 Generated with [Claude Code](https://claude.com/claude-code)

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `e18cc7713395`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.