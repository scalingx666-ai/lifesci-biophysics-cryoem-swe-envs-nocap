# Issue

**Title:** Fix argument order for GeodesicBAOABIntegrator

## Description

This fixes a problem with the API of `GeodesicBAOABIntegrator` so it can be used interchangeably with `BAOABIntegrator` and `LangevinIntegrator`.

At some point, we should add a test that makes sure all of our `LangevinIntegrators` can be invoked like:
```python
integrator = integrator_class(temperature, collision_rate, timestep)
```
to catch this issue in the future.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `4b93a1148719`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.