# Issue

**Title:** Pathwise distance between compartments

## Description

- deprecate `module.distance()`. It is replaced by `jx.morphology.distance_direct()`
- introduce `jx.morphology.distance_pathwise()`
- the new `distance_pathwise()` allows users to compute pathwise distance.

### Examples

Example 1: The following computes the pathwise distance between the zero-eth soma compartment and all other compartments. It then saves this distance in
`cell.nodes["path_dist_from_soma"]`.
```python
from jaxley.morphology import distance_pathwise

path_dists = distance_pathwise(cell.soma.branch(0).comp(0), cell)
cell.nodes["path_dist_from_soma"] = path_dists
```

Example 2: The following computes the pathwise distance between two compartments.
```python
from jaxley.morphology import distance_pathwise

dist = distance_pathwise(cell.branch(8).comp(2), cell.branch(2).comp(0))
```

Example 3: The following computes the direct (line of sight) distance between the zero-eth soma compartment and all other compartments. It then saves this distance in `cell.nodes["direct_dist_from_soma"]`.

```python
from jaxley.morphology import distance_direct

cell.compute_compartment_centers()  # necessary if you modified branch length.
direct_dists = distance_direct(cell.soma.branch(0).comp(0), cell)
cell.nodes["direct_dist_from_soma"] = direct_dists
```

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `3946e98720f9`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.