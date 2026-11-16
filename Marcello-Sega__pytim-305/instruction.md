# Issue

**Title:** Fixed memory leak

## Description

When instantiating an analysis such as
interface = pytim.ITIM(...)
multiple times, a memory leak occured.
This commit fixes that using a weakref.

This is to produce the error:

```
import MDAnalysis as mda 
import pytim
from pytim.datafiles import *

u = mda.Universe(WATER_GRO)
for i in range(0,2000):
    print i
    interface = pytim.ITIM(u, alpha=2., max_layers=4, molecular=True)
```


Here is where the error was found in ITIM:

```
        for nlayer, layer in enumerate(self._layers[
                0]):  # TODO should this be moved out of assign_layers?
            self._surfaces[nlayer] = Surface(self, options={'layer': nlayer})
```
Which is the penultimate block in itim.py

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `17dd7aab38fe`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.