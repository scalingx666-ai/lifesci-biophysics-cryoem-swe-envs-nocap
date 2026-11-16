# Issue

**Title:** Fixed problem with angle_indices of pytraj.topology.topology.Topology

## Description

When I called angle_indices of the pytraj.topology.topology.Topology,
I got the following error.

```
  File "pytraj/topology/topology.pyx", line 751, in pytraj.topology.topology.Topology.angle_indices.__get__
  File "/home/uchida/cnt/enhancer/utils/.venv/lib/python3.7/site-packages/numpy/core/_asarray.py", line 85, in asarray
    return array(a, dtype, copy=False, order=order)
TypeError: int() argument must be a string, a bytes-like object or a number, not 'NoneType'
```
I added a `return` statement at the end of `indeces` in pytraj.core.parameter_types.AtomType and also added a test.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `f12cc389b8e4`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.