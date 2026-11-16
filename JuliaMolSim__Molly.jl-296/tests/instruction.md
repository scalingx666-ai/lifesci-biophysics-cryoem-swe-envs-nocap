# Issue

**Title:** Fix shared-memory races in kernel_min_max! / kernel_min_max_triclinic!

## Description

`kernel_min_max!` and `kernel_min_max_triclinic!` (in `ext/MollyCUDAExt.jl`) compute each 32-atom block's bounding box with a shared-memory tree min/max over `log2(32)` levels. There is no barrier between levels. At level `p`, lane `i` (with `i % 2^p == 0`) reads slot `i - 2^(p-1)`, and a different lane wrote that slot at level `p - 1`. The code therefore depends on all lanes of the warp executing in lockstep. That is a data race:

- Since Volta, independent thread scheduling means lanes of a warp are not guaranteed to run in lockstep. Implicit warp-synchronous programming is explicitly unsupported.
- Without a barrier (and its memory fence), the CUDA memory model does not order these shared-memory accesses. The compiler may also reorder them or keep values in registers.

There is a similar problem in the remainder path of the same kernels. Lanes `1:r` write `r_smem`, and every lane writes the initial `xyz_min`/`xyz_max`. Lane 1 then reads and updates them with no barrier in between, so a lagging lane can overwrite lane 1's partial result with the initial value.

## Fix

- Add a `sync_warp()` after each tree level. Each block is exactly one warp (`threads=32`), so a warp barrier is enough and cheap. Within a single level there is no conflict: the slots being read (`≡ 2^(p-1) mod 2^p`) are never written in that level. The barrier sits outside the per-lane `if local_i % 2^p == 0` branch. The enclosing `if i <= n - r` branch is uniform across the block (`n - r` is a multiple of 32 and `threads=32`), so all 32 lanes reach every barrier. Lane 32 reads the final result from its own slot, which it wrote itself, so that read needs no extra sync.
- Add a `sync_threads()` before the lane-1 remainder scan.

I kept the existing shared-memory structure to keep the diff minimal. Rewriting it as a `shfl_xor_sync` butterfly, like the reduction already used elsewhere in this file, would also work and would drop the shared arrays. I'm happy to switch to that if you prefer.

## Possible related issue (not changed here)

The diagonal-tile loops (Part 3/4 of the force kernel, `for m in (lane + 1):32` with `opposites_sum[m, k, warpid] -= ...`) do a non-atomic read-modify-write on shared memory. Lane `L1` at iteration `t1` and lane `L2` at iteration `t2` touch the same slot whenever `L1 + t1 == L2 + t2`. This is only safe if lanes advance through the loop in lockstep, and nothing guarantees that: trip counts differ per lane, and `sum_pairwise_forces_gpu` is under a divergent `condition ? ... : ...`. The `slot = (lane + m) & 31` loops in the CLEAN/off-diagonal paths have the same cross-iteration pattern, although the `vote_any_sync` in each iteration probably reconverges the warp in practice. Fixing this properly needs a uniform 31-iteration loop with predication and a `sync_warp()` per iteration (or shuffles). That touches a hot path, so I've left it out of this PR.

## Testing

- On a Quadro RTX 4000 (Turing), I launched `kernel_min_max!` 200 times for each of N ∈ {1600, 1607} and D ∈ {2, 3}, covering both the full-block and remainder paths. Every result matched a CPU per-block min/max. I did not test `kernel_min_max_triclinic!` separately; its change is identical.
- I have not run the full test suite.

Found while surveying Molly's warp-level operations for a portable warp/subgroup API in KernelAbstractions.

🤖 Generated with [Claude Code](https://claude.com/claude-code)

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `310540f13440`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.