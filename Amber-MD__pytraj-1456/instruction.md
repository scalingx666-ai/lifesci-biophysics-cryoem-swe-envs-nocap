# Issue

**Title:** Write time

## Description

#1455 

Example
```python
In [15]: import pytraj as pt

In [16]: traj = pt.iterload('../cpptraj/test/FtuFabI.NAD.TCL.nc', 
                           '../cpptraj/test/FtuFabI.NAD.TCL.parm7')

In [17]: [f.time for f in traj]
Out[17]: 
[37031.0,
 37032.0,
 37033.0,
 37034.0,
 37035.0,
 37036.0,
 37037.0,
 37038.0,
 37039.0,
 37040.0]

In [18]: pt.write_traj('new.nc', traj, time=True)

In [19]: traj2 = pt.load('new.nc', traj.top)

In [20]: [f.time for f in traj2]
Out[20]: 
[37031.0,
 37032.0,
 37033.0,
 37034.0,
 37035.0,
 37036.0,
 37037.0,
 37038.0,
 37039.0,
 37040.0]
```

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `0dc27bbb96f4`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.