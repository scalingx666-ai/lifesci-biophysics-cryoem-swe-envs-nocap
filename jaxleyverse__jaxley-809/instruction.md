# Issue

**Title:** Fix jitting solvers with constant matrices broken under jit(vmap) on certain JAX versions

## Description

Between **jax/jaxlib ≈ 0.5.3 and 0.8.1**, the pattern below (used in the Jaxley solver) seems broken when jitting a `vmap` over several columns of a matrix that is a constant inside the function (i.e., not passed in as an argument):

```python
solves.at[i].set(voltages[i] + delta_t * constant_terms[i])
```

`jit(f)(B)` is fine but `jit(lambda: f(B))()` returns wrong values. See  /  (fixed in jaxlib ≥ 0.8.2).

**Minimal example:**
```python
import jax, jax.numpy as jnp

B = jax.random.normal(jax.random.PRNGKey(0), (10, 8))
idx = jnp.array([1, 3, 5, 7])

def col_fn(rhs):
    v = jnp.zeros(10).at[:10].set(rhs)
    c = jnp.zeros(10)
    return jnp.zeros(10).at[idx].set(v[idx] + 0.025 * c[idx])

vmap_fn = lambda M: jax.vmap(col_fn, in_axes=1, out_axes=1)(M)
print(jnp.max(jnp.abs(jax.jit(vmap_fn)(B) - jax.jit(lambda: vmap_fn(B))())))
```

This affects Jaxley’s implicit voltage solves (DHS, sparse, Stone) when applying them to each column of a fixed matrix `B` inside a jitted function.

**Fix:** 
write it as:
```python
solves = solves.at[i].set(voltages[i])
solves = solves.at[i].add((delta_t * constant_terms)[i])
```

**Added a test** that `jit(vmap)(B)` matches `jit(lambda: vmap(B))()`

Disclaimer: diagnosed and fixed with help of cursor.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `7a37da757d8c`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.