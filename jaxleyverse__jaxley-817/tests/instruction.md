# Issue

**Title:** Fix pumped ion registration during module composition

## Description

. Reported by @aslansd in #810.

When a Branch, Cell, or Network is built from constituents containing pumps,
the pump objects are inherited but `pumped_ions` is left empty. Integration
then fails when evaluating the pump's contribution to its ion concentration.

### Minimal reproduction

```python
import jaxley as jx
from jaxley.pumps import CaFaradayConcentrationChange

cell = jx.Cell()
cell.insert(CaFaradayConcentrationChange())
net = jx.Network([cell])
net.record("CaCon_i", verbose=False)

print(net.pumped_ions)
jx.integrate(net, t_max=0.1)
```

Before: `[]`, followed by `KeyError: 'CaCon_i'`.
After: `['CaCon_i']`, and integration succeeds.

### Fix

Register each inherited pump's `ion_name` in
`_gather_channels_from_constituents`, retaining one entry per ion.
Extend the existing module composition tests to compare voltage and calcium
concentration traces when pumps are inserted before or after assembly,
including repeated occurrences of the same ion across constituent cells
and differently named pumps.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `c5cf85720854`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.