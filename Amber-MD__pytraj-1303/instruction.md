# Issue

**Title:** ensure pytraj reproduce elec energy

## Description

try Niel's files:

``` python
import sander
import pytraj as pt
import parmed as pmd
# make default pme input
inp = sander.pme_input()
# update to your files
parm_file = 'prmtop'
rst7_file = 'inpcrd'
parm = pmd.load_file(parm_file, rst7_file)

with sander.setup(parm_file, parm.coordinates, parm.box, mm_options=inp):
    e, f = sander.energy_forces()
    print('from pysander: ',e.elec)

traj = pt.iterload(rst7_file, parm_file)
ene = pt.esander(traj, mm_options=sander.pme_input())
print('from pytraj: ', ene['elec'][0])
```

output

``` bash
from pysander:  -8225.698560432662
from pytraj:  -8225.69856043
```

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `1810f06ae7c3`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.