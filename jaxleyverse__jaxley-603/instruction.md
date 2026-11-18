# Issue

**Title:** Allow `.distance` to be jitted

## Description

The following does not work in `JAX`:
```python
@jit
def mytest(param):
    return np.sqrt(np.sum(param * np.asarray([3.0, 4.0])))
mytest(jnp.ones((1,)))
```

To make this work, one needs:
```python
@jit
def mytest(param):
    return jnp.sqrt(jnp.sum(param * np.asarray([3.0, 4.0])))
mytest(jnp.ones((1,)))
```

This caused our `.distance()` to break within a `jit`. This PR fixes it.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `7d286e848fbd`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.