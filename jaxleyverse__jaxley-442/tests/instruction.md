# Issue

**Title:** Use maximal ncomp per level of the solve, not the globally maximal ncomp

## Description

On the following simulation (which is chosen such that this PR gives a maximal performance boost), we get a speed up of 3x for compilation time and 12x for runtime. This is achieved by using the maximal number of compartments **within a level** for the voltage solve, instead of using the globally maximal number of compartments per branch.

```python
import time

import jaxley as jx
from jaxley.channels import Leak


depth = 7
parents = [-1] + [b // 2 for b in range(0, 2**depth - 2)]

comp = jx.Compartment()
branch1 = jx.Branch(comp, nseg=16)
branches = [branch1] + [jx.Branch(comp, nseg=1) for i in range(len(parents) - 1)]
cell = jx.Cell(branches, parents=parents)
cell.insert(Leak())

cell.delete_stimuli()
cell.delete_recordings()
cell.branch(0).comp(0).stimulate(jx.step_current(1, 1, 10.0, 0.025, 10.0))
cell.branch(1).comp(0).record()

def simulate(value):
    pstate = cell.data_set("Leak_gLeak", value, None)
    return jx.integrate(cell, param_state=pstate)

jitted_simulate = jit(simulate)

start_time = time.time()
v = jitted_simulate(jnp.asarray(0.002))
print(f"Compilation time: {time.time() - start_time:.4f}")

start_time = time.time()
v = jitted_simulate(jnp.asarray(0.003))
print(f"Run time:         {time.time() - start_time:.4f}")
```

Before this PR:
```shell
Compilation time: 4.5502
Run time:         0.0145
```

After this PR:
```shell
Compilation time: 1.7179  # 3x speed up
Run time:         0.0013  # 12x speed up
```

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `e78bb266f608`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.