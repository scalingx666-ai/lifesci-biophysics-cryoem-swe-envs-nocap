# Issue

**Title:** Solver `n_comp` scaling fix

## Description

### Problem

The backward pass through `jx.integrate` scales **O(n^2)** with the number of compartments on CPU, despite O(n) FLOPs. The root cause is that JAX's automatic VJP of `fori_loop` must carry and copy the full O(n) state tensor on every iteration, resulting in O(n^2) memory traffic. This makes gradient computation prohibitively slow for large morphologies (e.g. 700ms backward for 10,000 compartments vs 0.89ms forward — a 788x ratio).

### Solution

Add a `custom_jvp` to the tridiagonal solve. The key mathematical insight is that the gradient of a linear solve `A x = b` is itself a linear solve with the same matrix:

```
dx = A^{-1} (db - dA x)
```

This means the backward pass is a single O(n) tridiagonal solve rather than differentiating through the O(n)-step `fori_loop`. JAX automatically derives the VJP (reverse-mode) from the custom JVP rule.

### Changes

**`jaxley/solver_voltage.py`**
- Add `_make_dhs_solve()`: factory that creates a solve function with `@jax.custom_jvp`. The JVP computes `dA @ x` using precomputed child/parent index arrays, then solves `A^{-1}(db - dA x)` by calling the same solver.
- Add `_pad_comp_edges()`: pads ragged grouped edge arrays to dense numpy arrays (needed because levels can have different numbers of edges).
- Add `_get_dhs_solve()`: caches the solve function on the `solve_indexer` dict to avoid recreating it on every call. (Note: caching is currently semi-optimal and may be removable.)
- Simplify `step_voltage_implicit_with_dhs_solve()` to delegate to the cached solve function.
- Simplify `_comp_based_backsub_recursive_doubling()`: compute `num_recursive_steps` via `ceil(log2)`, use JAX array for `parent_jump` (enabling `parent_jump[parent_jump]` doubling), remove `n_nodes` parameter.

**`jaxley/modules/base.py`**
- In `_init_solver_jaxley_dhs_solve()`: precompute flat `all_children` and `all_parents` index arrays from `node_order_grouped` for the custom JVP's `dA @ x` computation.

**`jaxley/modules/network.py`**
- Same `all_children`/`all_parents` precomputation for the network-level solver init.

**`tests/test_solver.py`**
- Add `test_dhs_solve_handles_ragged_grouped_edges`: verifies correctness of the solve and its gradient against `np.linalg.solve` for both CPU and GPU paths, including ragged edge groupings.

**`tests/helpers.py`, `tests/test_graph.py`**
- Unrelated fixes: NaN-safe DataFrame comparison, dtype handling in graph tests, formatter-driven assert style changes.

### Benchmark results

#### ncomp scaling (single branch)

| ncomp  | Bwd main (ms) | Bwd this branch (ms) | Speedup |
|--------|----------------|-----------------------|---------|
| 10     | 0.04           | 0.04                  | 1x      |
| 1,000  | 9.01           | 0.48                  | **19x** |
| 10,000 | 700.40         | 3.19                  | **220x**|

The bwd/fwd ratio drops from 789x to 3.8x at 10,000 compartments.

#### Morphology (SWC) benchmarks — selected highlights

| Morphology                         | ncomp | Bwd main (ms) | Bwd this branch (ms) | Speedup |
|------------------------------------|-------|----------------|-----------------------|---------|
| morph_ca1_n120.swc                 | 4     | 5.71           | 0.84                  | **7x**  |
| morph_ca1_n120.swc                 | 16    | 39.49          | 1.52                  | **26x** |
| morph_allen_485574832.swc          | 16    | 19.34          | 1.10                  | **18x** |
| morph_retina_20161028_1.swc        | 16    | 39.68          | 1.49                  | **27x** |

Compilation times are unchanged. No impact on forward pass performance.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `fe98b2e2ad95`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.