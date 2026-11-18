# Issue

**Title:** `make_trainable` Padding Fix

## Description

This PR fixes a bug where `make_trainable` adds a `-1` padding on compartment
indices with the intention of having those indices skipped. The padding happens
in the below code snippet in `jaxley/modules/base.py`.

```python
# check if all shapes in comp_inds are the same. If not the case this means
# the groups in controlled_by_param have different sizes, i.e. due to different
# number of comps for two different branches. In this case we pad the smaller
# groups with -1 to make them the same size.
lens = np.array([inds.shape[0] for inds in comp_inds])
max_len = np.max(lens)
pad = lambda x: np.pad(x, (0, max_len - x.shape[0]), constant_values=-1)
if not np.all(lens == max_len):
    comp_inds = [
        pad(inds) if inds.shape[0] < max_len else inds for inds in comp_inds
    ]
```

The problem is that JAX uses `-1` (like Python) to indicate the last element of
an array. This means that trainable index groups can incorrectly index the last
compartment of the module.

This PR simply edits the JAX `.at[]` calls where the indices are used. They now
take an extra keyword argument `wrap_negative_indices=False`, which gives the
desired behaviour of the -1 indices.

Test cases are also added which fail on the current Jaxley version but pass
on the new version.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `db49cf3ea7b6`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.