# Issue

**Title:** Added new centre of mass and scippified, add property to access coords

## Description

example way to build specie indices and masses

```python
data = np.arange(0, 416).reshape(52, 8)
params = {'specie': None,
          'time_step': 2.0 * sc.Unit('fs'),
          'step_skip': 50 * sc.Unit('dimensionless'),
          'specie_indices': sc.array(dims=['atom','group_of_atoms'], values=data, unit=sc.Unit('dimensionless')),
          'masses': sc.array(dims=['group_of_atoms'], values=[1, 1, 3, 40,4,4,4,40])
          }


```

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `cad4cdf1155c`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.