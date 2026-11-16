# Issue

**Title:** Standardize integrator variable in ContextCache.

## Description

- Global variables can be standardized in `ContextCache` even without setter/getter.
- Per-DOF variables are ignored for the purpose of computing the integrator hash.
- Add standardization of `LangevinIntegrator` global variables.
- More global variables and attributes can be standardized by updating the class variable `COMPATIBLE_INTEGRATOR_ATTRIBUTES`

```python
openmmtools.cache.ContextCache.COMPATIBLE_INTEGRATOR_ATTRIBUTES.update({
    'my_integrator_global_variable': standard_value
})
```

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `7125729ef81b`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.