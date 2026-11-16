# Issue

**Title:** The admission estimate follows the largest unit and sees nested data

## Description

## Summary

.

**What went wrong.** The memory estimate that decides when a swarm item or a fan-out branch may start summed the files at the top level of its folder × 6. It was wrong both ways:
- **Too high:** a folder of six hyperspectral cubes was estimated at 13.3 GB, though an item runs its cubes one at a time and peaks at 4–8 GB. That held items that would have fitted.
- **Too low:** a raw instrument folder whose ~9 GB of hologram stacks sit in a subfolder was estimated at the 0.5 GB floor.

**What changes.** The estimate now:
- walks the folder **recursively**, counting only data files, still restricted by the item's `pattern`;
- takes the **largest unit**, not the sum, measured in memory (an `.npy` from its header, an HDF5 file from its largest dataset uncompressed, since compression hides about half);
- multiplies by `series_workers` when replays fan out;
- estimates a file inside a **raw-instrument folder** by its preparation (about 1.2 × one stack), wherever that folder sits, so a study bundle that holds one is estimated correctly too.

**Real data (the Holaris DyNO3 bundle):**

| folder | before | after | measured (from #724) |
|---|---|---|---|
| `hsi/` (6 cubes, 1.4 GB) | 13.28 GB | 5.55 GB | a peak of 4–8 GB |
| `mmzi/` (raw, 9.3 GB of stacks in a subfolder) | 0.50 GB | 2.55 GB | ~2.4 GB per stack in preparation |
| the whole bundle | 0.50 GB | 5.55 GB | — |
| `raman/` | 0.50 GB | 0.50 GB | — |

A first version estimated the whole bundle at 16.7 GB: it decided "raw" at the root only, so the nested hologram stack got the analysis factor. It now decides per file, and a test pins that shape.

---

## Technical details

- **`fanout._branch_mem_estimate`:**
  - `_data_files` (`rglob(pattern)` over `_DATA_SUFFIXES`, capped at 5,000 files);
  - `_in_memory_bytes` (`.npy` header via `mmap_mode="r"`; HDF5 by `visititems` for the largest dataset);
  - each file is classed raw if any folder from its own up to the branch root is `detect_raw_instrument`, with results cached;
  - result: `max(largest_analysis × 6 × workers (+ pool overhead above 100 MB), largest_raw × 1.2, floor)`.
  - It never raises, and the constants are unchanged.
- **`swarm._estimate`** passes `series_workers` through.
- **CLAUDE.md:** the coordinator rule.

**Tests** (`tests/test_mem_estimate.py`, 6, on real files):
- six cubes are estimated as one, and two workers as two;
- nested data is counted, and a pattern still restricts;
- HDF5 is estimated uncompressed;
- a raw folder by its preparation, and a bundle with a nested raw folder by unit kind;
- `swarm.capacity_plan` admits together two series items that the sum-based estimate kept apart.

`tests/test_resource_guards.py` still passes unchanged. Full suite against a `main` worktree: no branch-only failures.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `ff620304bd3f`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.