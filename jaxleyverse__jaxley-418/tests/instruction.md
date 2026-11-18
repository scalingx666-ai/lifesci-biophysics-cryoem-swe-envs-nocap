# Issue

**Title:** Allow different nseg per branch and add `jax.sparse` solver

## Description

# New features

### Different nseg for every branch

This PR enables that every branch can have a different number of compartments.
```python
comp = jx.Compartment()
branch1 = jx.Branch(comp, nseg=1)
branch2 = jx.Branch(comp, nseg=2)
branch3 = jx.Branch(comp, nseg=3)
cell = jx.Cell([branch1, branch2, branch3], parents=[-1, 0, 0])
cell.insert(HH())

cell.branch(1).comp(1).stimulate(jx.step_current(0.5, 1.0, 0.1, 0.025, 5.0))
cell.branch(0).comp(0).record()

voltages = jx.integrate(cell, delta_t=dt, voltage_solver="jax.sparse")
```

This is achieved by:
1) Building infrastructure to deal with a different number of compartments and
2) relying on `jax.experimental.sparse.spsolve` as a generic solver.


### Generic sparse voltage solver

With this PR, we are now supporting `jx.integrate(..., voltage_solver="jax.sparse")`. This uses the generic [sparse solver of JAX](https://jax.readthedocs.io/en/latest/_autosummary/jax.experimental.sparse.linalg.spsolve.html) for solving the voltage equations. It has the advantage that it has zero compile time and can more easily deal with different nseg, but the disadvantage that it does not support `vmap`. Unfortunately, the current voltage solvers (`jaxley.thomas` and `jaxley.stone`) need a different format for the sparse voltage solver than the `jax.sparse` solver. Therefore, we have to support both. This is achieved by:

1) Splitting `Module.init_morph()` into `Module._init_morph_jax_spsolve()` and `Module._init_morph_custom_spsolve()` (the latter is the one we already had).
2) Splitting `Module.init_conds()` into `Module._init_conds_jax_spsolve()` and `Module._init_conds_custom_spsolve()` (the latter is the one we already had).
3) Splitting `step_voltage_implicit()` (which lives in `jaxley.solver_voltage.py`) into `step_voltage_implicit_with_jax_spsolve()` and `step_voltage_implicit_with_custom_spsolve()`.


# Follow-up work

### Required future work before releasing
- Currently, `nseg` cannot be set after initialization. We should allow something like `cell.branch.set("nseg", 4)`.
- In a future PR, I will also allow using `jaxley.thomas` and `jaxley.stone` with different `nseg` per branch by masking out unused dimensions. This is done in #426 

### Optional follow-up work (probably after the next release)
- Raise an explicit error when `vmap` is used around the sparse jax solver.
- Consider making `jax.sparse` the default solver because of its low compile time.
- Benchmark the solvers and write a short tutorial about it.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `eb5c8208430d`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.