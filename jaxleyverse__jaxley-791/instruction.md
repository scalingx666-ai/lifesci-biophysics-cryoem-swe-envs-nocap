# Issue

**Title:** Fix Long Creations of Views

## Description

.

Note with this script 

```python
from pprint import pprint
import jaxley as jx

comp = jx.Compartment()
branch = jx.Branch(comp, ncomp=2)
cell = jx.Cell(branch, parents=[-1, 0, 0, 1, 1])
net = jx.Network([cell] * 5)

print("CELL0 XYZR:")
pprint(net.cell(0).xyzr)
print("CELL0-BRANCH0 XYZR:")
pprint(net.cell(0).branch(0).xyzr)
print("CELL0-BRANCH1 XYZR:")
pprint(net.cell(0).branch(1).xyzr)
```

one gets the output,

```
CELL0 XYZR:
[array([[ 0.,  0.,  0., nan],
       [20.,  0.,  0., nan]]),
 array([[ 20.        ,   0.        ,   0.        ,          nan],
       [ 31.76343395, -16.17472169,   0.        ,          nan]]),
 array([[20.        ,  0.        ,  0.        ,         nan],
       [31.76343395, 16.17472169,  0.        ,         nan]]),
 array([[ 31.76343395, -16.17472169,   0.        ,          nan],
       [ 51.16628396, -21.02543419,   0.        ,          nan]]),
 array([[ 31.76343395, -16.17472169,   0.        ,          nan],
       [ 51.16628396, -11.32400919,   0.        ,          nan]])]
CELL0-BRANCH0 XYZR:
[array([[ 0.,  0.,  0., nan],
       [20.,  0.,  0., nan]])]
CELL0-BRANCH1 XYZR:
[array([[ 20.        ,   0.        ,   0.        ,          nan],
       [ 31.76343395, -16.17472169,   0.        ,          nan]])]
```

Is this the type of behaviour you were looking for @michaeldeistler ?

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `d2cefa46839b`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.