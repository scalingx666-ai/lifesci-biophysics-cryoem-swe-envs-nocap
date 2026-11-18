# Issue

**Title:** Exponential Euler and forward Euler solvers

## Description

This PR implements an exponential Euler solver, and it implements forward Euler also for branched morphologies. Thanks to Amrith Lothikar for discussions!

### API

Most simple API for running exponential Euler
```python
jx.integrate(cell, solver="exp_euler")
```

API for pre-computing the diffusion terms (and then not computing them during `jx.integrate()`):
```python
cell.customize_solver_exp_euler(
    exp_euler_transition=cell.build_exp_euler_transition_matrix(delta_t)
)
jx.integrate(cell, solver="exp_euler")
```
This can be useful if the diffusion terms (radius, length, ra, C) are the same across many simulations.

### Major limitations

The biggest limitation is that the solver does not yet understand the concept of independent neurons. That is, it treats networks as one large cell and performs matrix-vector multiplications, instead of block-diagonal matrix-vector multiplications (or even vector-vector for networks of single compartment).

### Benchmark

This solver requires to solve a matrix exponential upfront (only once for a given `axial_resistivity`, `length`, `radius`), and then requires _one_ matrix vector product at every integration step. Thus:
- this solver can be very performant on GPU.
  - If the matrix exponential can be pre-computed:
    - For single cells and `batchsize=1`, I get ~10x speedups. 
    - For `batchsize=10_000`, I get ~2x speedups.
  - If the matrix exponential cannot be pre-computed, but it is the same for every element in the entire batch (e.g., because the batch vectorizes across stimuli). This case requires additional effort from the user (see below):
    - The additional cost of pre-computing the matrix is small.
  - If the matrix exponential cannot be pre-computed and it is different for every element in the batch:
    - For small morphologies (~500 compartments), the matrix exponential takes as long as ~1 second of simulation.
    - For large morphologies (>1000 compartments), the cost of the matrix exponential becomes excessive and the solver becomes essentially unusable.
- the solver is very slow on CPU (around 10x slow-down).

### Summary

Another benefit is that this solver produces more accurate solutions than backward Euler, especially for very large `delta_t`. Even at `dt=1.0 ms`, I get somewhat good looking voltage traces and spikes.

Overall, I think this PR will allow adding some scale by making GPU runs faster (especially if the transition matrix can be precomputed), and potentially allowing a larger `delta_t`, at least for initial explorations.


### Appendix: How to implement the same exp euler matrix for every batch element

```python
def simulate(amp, euler_matrix):
    cell.customize_solver_exp_euler(exp_euler_transition=euler_matrix)
    dstate = cell.branch(0).comp(0).data_stimulate(jx.step_current(100.0, 840.0, amp, delta_t, t_max))
    return jx.integrate(cell, data_stimuli=dstate, solver="exp_euler")

vmapped_simulate = vmap(simulate, in_axes=(0, None))

def loss(ra, amps):
    pstate = cell.data_set("axial_resistivity", ra, None)
    cell.to_jax()
    all_params = cell.get_all_parameters(pstate)
    axial_conductances = all_params["axial_conductances"]["v"]
    euler_matrix = cell.build_exp_euler_transition_matrix(
        delta_t=delta_t, axial_conductances=axial_conductances
    )
    return vmapped_simulate(amps, euler_matrix)

jitted_loss = jit(loss)

start_time = time.time()
ra = jnp.asarray(np.random.rand() * 1000)
amps = jnp.asarray(np.random.rand(num_vmap))
v = jitted_loss(ra, amps)
v.block_until_ready()
logging.info(f"EXP Compile Time: {time.time() - start_time}")
```

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `000b57768f0d`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.