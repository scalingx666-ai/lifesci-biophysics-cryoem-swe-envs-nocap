# Issue

**Title:** GITIM only working with MDAnalysis. Changed data type for self._layer…

## Description

GITIM only working with MDAnalysis. Changed data type for self._layers in GITIM from type(universe.atoms) to self.universe.atoms[0].__class__, which is the same as in ITIM. This does not appear to break anything. At least the DPC micelle example with MDAnalysis gives the same number of interfacial atoms after the fix and an mdtraj version of the DPC micelle example also gives the same number of interfacial atoms after the fix. Here is the mdtraj version of the DPC micelle example:

```python
import mdtraj
import pytim
from   pytim.datafiles import *

t = mdtraj.load_pdb(MICELLE_PDB)
g = t.top.select("resname=='DPC'")

interface = pytim.GITIM(t, group=g, molecular=False, symmetry='spherical', alpha=2.5)
layer = interface.layers[0]
print repr(layer)
```

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `2d344eb0591a`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.