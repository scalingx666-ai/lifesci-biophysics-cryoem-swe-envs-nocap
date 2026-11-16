# Issue

**Title:** Reuse exactly-sized free blocks in the custom GPU allocator

## Description

, reported by @fengxiaoshuai.

`_getFirstSuitedFree()` skips a free block whose size is *exactly* the requested size:

```cpp
while (a != NULL && ( a->size <= size || ! a->free ) )
```

That makes the `curAlloc->size == size` branch in `alloc()` unreachable, and a request the pool can satisfy exactly instead aborts with `CRITICAL(ERRGPUCAOOM)`. It also stops a freed block from being reused by a request of the same size, which fragments the pool. Since requests are rounded up to `alignmentSize` first, exact fits are common.

Changed `<=` to `<`. The HIP port has an identical copy of the function, so both are updated.

Not compiled against CUDA/HIP here (the build workflow is CPU-only, so it does not cover these headers) — checked by running the free-list logic standalone with the CUDA calls stubbed out.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `d61cfdcbf96e`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.