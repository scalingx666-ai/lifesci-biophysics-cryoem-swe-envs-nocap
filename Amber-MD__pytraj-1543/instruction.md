# Issue

**Title:** Fix disappearing forces when slicing

## Description

When iterating over a sliced trajectory which contains forces, the forces were not included in the new sliced trajectory, this fixes that.

Before:

```
>>> import pytraj as pt
>>> traj = pt.iterload('03_Prod.nc','parm7')
>>> traj[0:5][0].force
>>>
```

After:

```
>>> import pytraj as pt
>>> traj = pt.iterload('03_Prod.nc','parm7')
>>> traj[0:5][0].force
array([[ -3.07928634,   0.68935108,  -0.12736608],
       [  7.64932537, -19.16348267, -16.6106205 ],
       [ -2.71725702,  -1.42881477,   8.08388519],
       ...,
       [  9.62166405,  -1.84283781,  -4.25340939],
       [  0.85549378,   5.10778189, -12.62469864],
       [ -8.67284012,   3.77264452,   8.0937624 ]])
```

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `89227617c936`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.