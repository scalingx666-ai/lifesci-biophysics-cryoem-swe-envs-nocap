# Issue

**Title:** Avoid copying the neighbor list when the pair estimate is exceeded

## Description

`neighborlist` preallocates its output for the number of pairs expected at uniform density. If the real count is above that estimate, even by a single pair, the whole output list gets copied:

- **Parallel:** `reduce_output!` called `resize!` on the full output list, which copies all elements, although only the first `output.n` of them hold data (none, in a fresh `neighborlist` call).
- **Serial:** pairs are pushed directly into the output list, so `push!` growth copied it.

For uniform systems the real count is just above the estimate about half the time. For example, with 100k particles at density 0.1 and a 12 Å cutoff, the cubic cell gave 36,192,561 pairs against an estimate of 36,190,785. Copying that 870 MB list took ~230 ms, against ~100 ms for the rest of the computation.

## Changes

Both changes are in `src/internals/neighborlist.jl`:
- `reduce_output!` shrinks the list to `output.n` before growing it, so only the elements that hold data are copied.
- `_sizehint_neighbor_lists!` gives the output list a 5% margin over the estimate, as the per-thread lists already have. Capacity that is never written costs only virtual memory, and the final `resize!` keeps the capacity, so repeated calls still don't allocate.

## Benchmark

`neighborlist`, density 0.1, best of 5 runs, threads pinned to P-cores, times in seconds:

| System | 1 thread before → after | 8 threads before → after |
|---|---|---|
| 100k, 5 Å, cubic | 0.069 → 0.053 | 0.034 → 0.013 |
| 1M, 5 Å, cubic | 0.510 → 0.508 | 0.132 → 0.133 |
| 100k, 12 Å, cubic | 0.587 → 0.372 | 0.386 → 0.106 |
| 100k, 5 Å, triclinic | 0.070 → 0.054 | 0.033 → 0.013 |
| 1M, 5 Å, triclinic | 0.525 → 0.546 | 0.127 → 0.140 |
| 100k, 12 Å, triclinic | 0.378 → 0.377 | 0.104 → 0.109 |

Systems whose pair count was already under the estimate (the 1M rows and 12 Å triclinic) don't change, apart from noise: repeated 8-thread runs at 12 Å varied between 0.12 and 0.19 s.

Clustered systems with many more pairs than the uniform estimate still overflow the 5% margin. In those, the parallel reduction no longer copies, but the serial `push!` growth remains.

All tests pass, including the allocation tests (`BUILD_IS_PRODUCTION_BUILD=true`).

🤖 Generated with [Claude Code](https://claude.com/claude-code)

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `6097af5e176e`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.