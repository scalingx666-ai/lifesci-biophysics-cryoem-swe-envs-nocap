# Issue

**Title:** Fix synapse type filtering in Module.record()

## Description

.

### Problem

When recording a synaptic state or current, `Module.record()` collects
`index_within_type` from every synapse type in the selected view. Indices
from unrelated types can therefore be registered for the requested variable,
producing extra recordings and potentially duplicating the last trace.

Minimal reproduction:

```python
import jaxley as jx
from jaxley.synapses import IonotropicSynapse

net = jx.Network([jx.Cell()] * 3)
jx.fully_connect(net.cell(0), net.cell(2), IonotropicSynapse("syn1"))
jx.fully_connect(net.cell([0, 1]), net.cell(2), IonotropicSynapse("syn2"))

net.record("i_syn1", verbose=False)
print(net.recordings.rec_index.tolist())
```

There is only one `syn1` connection, so the expected result is `[0]`.
Before this fix, the result is `[0, 1]`.

This affects recordings from networks or views containing multiple synapse
types, including downstream analysis of those traces.

### Fix

Identify the synapse type owning the requested state or current, then filter
the selected edges by `type_ind` before collecting `index_within_type`.

The change preserves the selected view and existing duplicate removal.
It uses mechanism metadata, so currents do not need to be added to the
edges dataframe, and NaN state values do not exclude valid connections.

Only recording setup changes; the integration loop is unchanged.

The PR includes a changelog entry and 12 regression test cases covering
states and currents, mixed views, type insertion order, custom state names,
NaN values, and existing recording behavior.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `968b998d0ee8`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.