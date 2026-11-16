# Issue

**Title:** Fix Topology double-loading for HDF5 and MOL2 (+ remove false-positive UserWarning)

## Description

Currently, loading `.h5` and `.mol2` files with `mdtraj.load()` will always return a `UserWarning`, even though no `top` kwarg is passed. This is because the topology is loaded twice for these file types (these loaders don't take the `top` argument), and the topology from the first load triggers the `UserWarning` right before the second successful topology load.

More precisely, the two topology loads happen 
A) once during [topology parsing](https://github.com/mdtraj/mdtraj/blob/main/mdtraj/core/trajectory.py#L398) and 
B) once during this [try/except block](
https://github.com/mdtraj/mdtraj/blob/main/mdtraj/core/trajectory.py#L442-L444) where topology is discarded (because a `top` argument is given) and the topology is reread again. 


This can lead to a large performance hit for large system topologies. In fact there is a huge comment block/TODO dedicated to this: https://github.com/mdtraj/mdtraj/blob/main/mdtraj/core/trajectory.py#L424-L431 . We might want to implement the TODO eventually, but that will require some planning + going through all formats. This is the "quick fix" that alleviates the performance hit, by skipping the first load only for these file types.

====

BEFORE:

```
In [1]: import mdtraj

In [2]: a = mdtraj.load('data/traj.h5')
/Users/user/mdtraj/mdtraj/core/trajectory.py:440: UserWarning: top= kwargs ignored since this file parser does not support it
  warnings.warn("top= kwargs ignored since this file parser does not support it")
In [3]: a = mdtraj.load('data/traj.h5', top='data/traj.h5')
/Users/user/mdtraj/mdtraj/core/trajectory.py:440: UserWarning: top= kwargs ignored since this file parser does not support it
  warnings.warn("top= kwargs ignored since this file parser does not support it")
```

AFTER:
```
In [1]: import mdtraj

In [2]: a = mdtraj.load('data/traj.h5')

In [3]: a = mdtraj.load('data/traj.h5', top='data/traj.h5')
/Users/user/mdtraj/mdtraj/core/trajectory.py:444: UserWarning: top= kwargs ignored since this file parser does not support it
  warnings.warn("top= kwargs ignored since this file parser does not support it")
```

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `22544580ef71`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.