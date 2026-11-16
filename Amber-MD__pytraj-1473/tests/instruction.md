# Issue

**Title:** Example of passing a trajectory list to pytraj.compute

## Description

#1461 
```python
    >>> class TrajectoryList:
    ...     def __init__(self, trajs):
    ...         self._trajs = trajs
    ...
    ...     @property
    ...     def top(self):
    ...         # assume all trajs have the same topology
    ...         return self._trajs[0].top
    ...
    ...     def __iter__(self):
    ...         for frame in chain(self._trajs):
    ...             yield frame
    >>> data = pt.compute(['rmsd @CA', 'radgyr'], TrajectoryList([traj0, traj1]))
```

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `05ba9055e4bf`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.