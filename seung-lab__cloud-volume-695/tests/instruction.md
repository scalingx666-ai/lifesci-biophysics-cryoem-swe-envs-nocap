# Issue

**Title:** fix: account for numpy array buffers in LRU sizing

## Description

## Problem

A byte-counted LRU (`size_in_bytes=True`) holding decoded **raw** chunks never evicts — memory grows ~one decoded chunk per read until the worker OOMs, and `max_cache_bytes` has no effect. Reported multiple times; most recently a 40 GiB-on-a-12-GiB-workload mystery on a single-proc, raw float32, no-stack, no-intermediaries run.

## Root cause

`getsizeof` in `lru.py` relied on `sys.getsizeof`, which only reports a numpy array's data buffer when the array **owns** it (`base is None`):

- Decoded raw chunks come from `decode_raw` → `np.frombuffer(bytestring, ...).reshape(...)` (`chunks.py:354`), a **view** whose `.base` is the source bytes.
- The single-process read path stores it directly as `("numpy", img3d)` (`image/rx.py:627`).
- `sys.getsizeof` returns only the ~150-byte object header, even though the full decompressed buffer stays alive via `.base`.

So the LRU's `nbytes` counter sits near zero no matter how many 100+ MB arrays it holds → `is_oversized()` never trips → the cache accumulates every chunk the worker ever reads.

Verified on numpy 2.3.5: a 10.5 MB frombuffer view was charged **246 bytes**.

The other store sites (`repopulate_lru_from_shm`, the upload path) use `np.copy(..., order="F")` — owning arrays — so they were accounted correctly; only the single-proc raw read path leaked. This matches the reproduction (`"same"` encoding stays bounded; `cache_bytes_limit` does nothing; malloc/jemalloc tuning doesn't help — it's genuinely-live cache, not fragmentation).

## Fix

Charge `ndarray`s their `.nbytes` explicitly in `getsizeof`, so the byte cap and eviction work regardless of view-vs-owner. No extra copy at store time.

## Verification

- Existing `test/test_lru.py` passes.
- Manual check: inserting 30× 10.5 MB raw views into a 30 MB-capped LRU now evicts down to 2 entries (~21 MB); previously it would hold all 30.

🤖 Generated with [Claude Code](https://claude.com/claude-code)

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `1e3fb63f9bbf`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.