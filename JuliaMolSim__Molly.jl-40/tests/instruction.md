# Issue

**Title:** Update adjust_bounds by removing iteration

## Description

The `adjust_bounds` is optimized into a one-line using the `floor` in base julia.
Here is the simple benchmark.

benchamarked julia version : 1.6.1

```julia
julia> @time adjust_bounds(90001.0,3.0)
  0.000029 seconds
1.0

julia> @time adjust_bounds_new(90001.0,3.0) # this is the function proposed
  0.000000 seconds
1.0
```

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `32c3596c779c`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.